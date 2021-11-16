# How-to-guide Tilemap

## 무엇을 하려고 하는가?

* Tilemap 제작 및 Rule Tile관련해서는 tutorial에서 설명하고, Editor를 조작하는 성향이 강하기 때문에, 이번 How-to-guide에서는 Procedural pattern Tilemap에 관련한 Script들에 대한 문서들을 작성합니다.
* 아래의 URL의 Blog를 따라 정리했습니다.

{% embed url="https://blogs.unity3d.com/kr/2018/05/29/procedural-patterns-you-can-use-with-tilemaps-part-i/" %}
Procedural Pattern Tilemap blog - 1
{% endembed %}

{% embed url="https://blogs.unity3d.com/kr/2018/06/07/procedural-patterns-to-use-with-tilemaps-part-ii/" %}
Procedural Pattern Tilemap blog - 2
{% endembed %}

* 좀 더 자세한 설명은 아래의 해설문서(Explanation)에 넣도록 하겠습니다.

{% content-ref url="../../explanation/unity/explanation-tilemap.md" %}
[explanation-tilemap.md](../../explanation/unity/explanation-tilemap.md)
{% endcontent-ref %}

## Generate Array

```csharp
public static int[,] GenerateArray(int width, int height, bool empty)
{
    int[,] map = new int[width, height];
    for (int x = 0; x < map.GetUpperBound(0); x++)
    {
        for (int y = 0; y < map.GetUpperBound(1); y++)
        {
            if (empty)
            {
                map[x, y] = 0;
            }
            else
            {
                map[x, y] = 1;
            }
        }
    }
    return map;
}
```

## Render Map

```csharp
public static void RenderMap(int[,] map, Tilemap tilemap, TileBase tile)
{
    //Clear the map (ensures we dont overlap)
    tilemap.ClearAllTiles(); 
    //Loop through the width of the map
    for (int x = 0; x < map.GetUpperBound(0) ; x++) 
    {
        //Loop through the height of the map
        for (int y = 0; y < map.GetUpperBound(1); y++) 
        {
            // 1 = tile, 0 = no tile
            if (map[x, y] == 1) 
            {
                tilemap.SetTile(new Vector3Int(x, y, 0), tile); 
            }
        }
    }
}
```

## Update Map

```csharp
public static void UpdateMap(int[,] map, Tilemap tilemap) //Takes in our map and tilemap, setting null tiles where needed
{
    for (int x = 0; x < map.GetUpperBound(0); x++)
    {
        for (int y = 0; y < map.GetUpperBound(1); y++)
        {
            //We are only going to update the map, rather than rendering again
            //This is because it uses less resources to update tiles to null
            //As opposed to re-drawing every single tile (and collision data)
            if (map[x, y] == 0)
            {
                tilemap.SetTile(new Vector3Int(x, y, 0), null);
            }
        }
    }
}
```

## Perlin Noise

```csharp
public static int[,] PerlinNoise(int[,] map, float seed)
{
    int newPoint;
    //Used to reduced the position of the Perlin point
    float reduction = 0.5f;
    //Create the Perlin
    for (int x = 0; x < map.GetUpperBound(0); x++)
    {
        newPoint = Mathf.FloorToInt((Mathf.PerlinNoise(x, seed) - reduction) * map.GetUpperBound(1));
 
        //Make sure the noise starts near the halfway point of the height
        newPoint += (map.GetUpperBound(1) / 2); 
        for (int y = newPoint; y >= 0; y--)
        {
            map[x, y] = 1;
        }
    }
    return map;
}
```

## Perlin Noise Smooth

```csharp
public static int[,] PerlinNoiseSmooth(int[,] map, float seed, int interval)
{
    //Smooth the noise and store it in the int array
    if (interval > 1)
    {
        int newPoint, points;
        //Used to reduced the position of the Perlin point
        float reduction = 0.5f;
 
        //Used in the smoothing process
        Vector2Int currentPos, lastPos; 
        //The corresponding points of the smoothing. One list for x and one for y
        List<int> noiseX = new List<int>();
        List<int> noiseY = new List<int>();
 
        //Generate the noise
        for (int x = 0; x < map.GetUpperBound(0); x += interval)
        {
            newPoint = Mathf.FloorToInt((Mathf.PerlinNoise(x, (seed * reduction))) * map.GetUpperBound(1));
            noiseY.Add(newPoint);
            noiseX.Add(x);
        }
 
        points = noiseY.Count;
        
        //Start at 1 so we have a previous position already
        for (int i = 1; i < points; i++) 
        {
            //Get the current position
            currentPos = new Vector2Int(noiseX[i], noiseY[i]);
            //Also get the last position
            lastPos = new Vector2Int(noiseX[i - 1], noiseY[i - 1]);

            //Find the difference between the two
            Vector2 diff = currentPos - lastPos;

            //Set up what the height change value will be 
            float heightChange = diff.y / interval;
            //Determine the current height
            float currHeight = lastPos.y;

            //Work our way through from the last x to the current x
            for (int x = lastPos.x; x < currentPos.x; x++)
            {
                for (int y = Mathf.FloorToInt(currHeight); y > 0; y--)
                {
                    map[x, y] = 1;
                }
                currHeight += heightChange;
            }
        }
    }
     else
        {
            //Defaults to a normal Perlin gen
            map = PerlinNoise(map, seed);
        }

        return map;
```

## Random Walk

```csharp
public static int[,] RandomWalkTop(int[,] map, float seed)
{
    //Seed our random
    System.Random rand = new System.Random(seed.GetHashCode()); 

    //Set our starting height
    int lastHeight = Random.Range(0, map.GetUpperBound(1));
        
    //Cycle through our width
    for (int x = 0; x < map.GetUpperBound(0); x++) 
    {
        //Flip a coin
        int nextMove = rand.Next(2);

        //If heads, and we aren't near the bottom, minus some height
        if (nextMove == 0 && lastHeight > 2) 
        {
            lastHeight--;
        }
        //If tails, and we aren't near the top, add some height
        else if (nextMove == 1 && lastHeight < map.GetUpperBound(1) - 2) 
        {
            lastHeight++;
        }

        //Circle through from the lastheight to the bottom
        for (int y = lastHeight; y >= 0; y--) 
        {
            map[x, y] = 1;
        }
    }
    //Return the map
    return map; 
}
```

## Random Walk Top Smoothed

```csharp
public static int[,] RandomWalkTopSmoothed(int[,] map, float seed, int minSectionWidth)
{
    //Seed our random
    System.Random rand = new System.Random(seed.GetHashCode());

    //Determine the start position
    int lastHeight = Random.Range(0, map.GetUpperBound(1));

    //Used to determine which direction to go
    int nextMove = 0;
    //Used to keep track of the current sections width
    int sectionWidth = 0;

    //Work through the array width
    for (int x = 0; x <= map.GetUpperBound(0); x++)
    {
        //Determine the next move
        nextMove = rand.Next(2);

        //Only change the height if we have used the current height more than the minimum required section width
        if (nextMove == 0 && lastHeight > 0 && sectionWidth > minSectionWidth)
        {
            lastHeight--;
            sectionWidth = 0;
        }
        else if (nextMove == 1 && lastHeight < map.GetUpperBound(1) && sectionWidth > minSectionWidth)
        {
            lastHeight++;
            sectionWidth = 0;
        }
        //Increment the section width
        sectionWidth++;

        //Work our way from the height down to 0
        for (int y = lastHeight; y >= 0; y--)
        {
            map[x, y] = 1;
        }
    }

    //Return the modified map
    return map;
}
```

## Cellular Automata

```csharp
public static int[,] GenerateCellularAutomata(int width, int height, float seed, int fillPercent, bool edgesAreWalls)
{
    //Seed our random number generator
    System.Random rand = new System.Random(seed.GetHashCode());
 
    //Initialise the map
    int[,] map = new int[width, height];
 
    for (int x = 0; x < map.GetUpperBound(0); x++)
    {
        for (int y = 0; y < map.GetUpperBound(1); y++)
        {
            //If we have the edges set to be walls, ensure the cell is set to on (1)
            if (edgesAreWalls && (x == 0 || x == map.GetUpperBound(0) - 1 || y == 0 || y == map.GetUpperBound(1) - 1))
            {
                map[x, y] = 1;
            }
            else
            {
                //Randomly generate the grid
                map[x, y] = (rand.Next(0, 100) < fillPercent) ? 1 : 0; 
            }
        }
    }
    return map;
}
```

## Moore Neighbourhood

{% tabs %}
{% tab title="Method 1" %}
```csharp
static int GetMooreSurroundingTiles(int[,] map, int x, int y, bool edgesAreWalls)
{
    /* Moore Neighbourhood looks like this ('T' is our tile, 'N' is our neighbours)
     * 
     * N N N
     * N T N
     * N N N
     * 
     */
 
    int tileCount = 0;       
    
    for(int neighbourX = x - 1; neighbourX <= x + 1; neighbourX++)
    {
        for(int neighbourY = y - 1; neighbourY <= y + 1; neighbourY++)
        {
            if (neighbourX >= 0 && neighbourX < map.GetUpperBound(0) && neighbourY >= 0 && neighbourY < map.GetUpperBound(1))
            {
                //We don't want to count the tile we are checking the surroundings of
                if(neighbourX != x || neighbourY != y) 
                {
                    tileCount += map[neighbourX, neighbourY];
                }
            }
        }
    }
    return tileCount;
}
```
{% endtab %}

{% tab title="Method 2" %}
```csharp
C#
public static int[,] SmoothMooreCellularAutomata(int[,] map, bool edgesAreWalls, int smoothCount)
{
	for (int i = 0; i < smoothCount; i++)
	{
		for (int x = 0; x < map.GetUpperBound(0); x++)
		{
			for (int y = 0; y < map.GetUpperBound(1); y++)
			{
				int surroundingTiles = GetMooreSurroundingTiles(map, x, y, edgesAreWalls);

				if (edgesAreWalls && (x == 0 || x == (map.GetUpperBound(0) - 1) || y == 0 || y == (map.GetUpperBound(1) - 1)))
				{
                    //Set the edge to be a wall if we have edgesAreWalls to be true
					map[x, y] = 1; 
				}
                //The default moore rule requires more than 4 neighbours
				else if (surroundingTiles > 4)
				{
					map[x, y] = 1;
				}
				else if (surroundingTiles < 4)
				{
					map[x, y] = 0;
				}
			}
		}
	}
    //Return the modified map
    return map;
}
```
{% endtab %}
{% endtabs %}

## von Neumann Neighbourhood

{% tabs %}
{% tab title="Method 1" %}
```csharp
static int GetVNSurroundingTiles(int[,] map, int x, int y, bool edgesAreWalls)
{
	/* von Neumann Neighbourhood looks like this ('T' is our Tile, 'N' is our Neighbour)
	* 
	*   N 
	* N T N
	*   N
	*   
    */
     
	int tileCount = 0;

    //Keep the edges as walls
    if(edgesAreWalls && (x - 1 == 0 || x + 1 == map.GetUpperBound(0) || y - 1 == 0 || y + 1 == map.GetUpperBound(1)))
    {
        tileCount++;
    }

    //Ensure we aren't touching the left side of the map
	if(x - 1 > 0)
	{
		tileCount += map[x - 1, y];
	}

    //Ensure we aren't touching the bottom of the map
	if(y - 1 > 0)
	{
		tileCount += map[x, y - 1];
	}

    //Ensure we aren't touching the right side of the map
	if(x + 1 < map.GetUpperBound(0)) 
	{
		tileCount += map[x + 1, y];
	}

    //Ensure we aren't touching the top of the map
	if(y + 1 < map.GetUpperBound(1)) 
	{
		tileCount += map[x, y + 1];
	}

	return tileCount;
}
```
{% endtab %}

{% tab title="Method 2" %}
```csharp
public static int[,] SmoothVNCellularAutomata(int[,] map, bool edgesAreWalls, int smoothCount)
{
	for (int i = 0; i < smoothCount; i++)
	{
		for (int x = 0; x < map.GetUpperBound(0); x++)
		{
			for (int y = 0; y < map.GetUpperBound(1); y++)
			{
				//Get the surrounding tiles
				int surroundingTiles = GetVNSurroundingTiles(map, x, y, edgesAreWalls);

				if (edgesAreWalls && (x == 0 || x == map.GetUpperBound(0) - 1 || y == 0 || y == map.GetUpperBound(1)))
				{
                    //Keep our edges as walls
					map[x, y] = 1; 
				}
                //von Neuemann Neighbourhood requires only 3 or more surrounding tiles to be changed to a tile
				else if (surroundingTiles > 2) 
				{
					map[x, y] = 1;
				}
				else if (surroundingTiles < 2)
				{
					map[x, y] = 0;
				}
			}
		}
	}
    //Return the modified map
    return map;
}
```
{% endtab %}
{% endtabs %}
