# How-to-guide Tilemap

## 무엇을 하려고 하는가?

* Tilemap 제작 및 Rule Tile관련해서는 tutorial에서 설명하고, Editor를 조작하는 성향이 강하기 때문에, 이번 How-to-guide에서는 Procedural pattern Tilemap에 관련한 Script들에 대한 문서들을 작성합니다.
* 아래의 URL의 Blog를 따라 정리했습니다.

[https://blogs.unity3d.com/kr/2018/05/29/procedural-patterns-you-can-use-with-tilemaps-part-i/](https://blogs.unity3d.com/kr/2018/05/29/procedural-patterns-you-can-use-with-tilemaps-part-i/)                               [https://blogs.unity3d.com/kr/2018/06/07/procedural-patterns-to-use-with-tilemaps-part-ii/](https://blogs.unity3d.com/kr/2018/06/07/procedural-patterns-to-use-with-tilemaps-part-ii/)

* 공
* 좀 더 자세한 설명은 아래의 해설문서\(Explanation\)에 넣도록 하겠습니다.

{% page-ref page="../../explanation/unity/explanation-tilemap.md" %}

## Generate Array

```text
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

```text
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

```text
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

```text
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

```text
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
```





