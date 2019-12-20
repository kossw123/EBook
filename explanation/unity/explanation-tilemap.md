---
description: Explanation Tilemap
---

# Explanation Tilemap

Procedural Pattern Tilemap

{% tabs %}
{% tab title="What is this blog post about?" %}
We’ll take a look at some of the most common methods of creating a procedural world, and a couple of custom variations that I have created.  Here’s an example of what you may be able to create after reading this article. Three algorithms are working together to create one map, using a [Tilemap](https://docs.unity3d.com/Manual/class-Tilemap.html) and a [RuleTile](https://github.com/Unity-Technologies/2d-extras):

Procedural World를 만들기 위한 일반적인 방법과, 변형적으로 추가한 몇가지 방법들을 소개합니다.다음은 아래의 기사를 읽은 후 만들 수 있는 몇가지 예입니다. 여기서는 Tilemap, Rule Tile을 사용한 세가지의 알고리즘이 하나의 맵을 만듭니다.

When we’re generating a map with any of the algorithms, we will receive an int array which contains all of the new data. We can then take this data and continue to modify it or render it to a tilemap.

알고리즘을 사용하여 맵을 만든다면, 우리는 새로운 데이터가 포함이 된 int형 배열을 받게됩니다.   그런다음 데이터를 가져와서 계속 수정하거나 하나의 Tilemap으로 만들수 있습니다.

Good to know before you read further:

1. The way we distinguish between what’s a tile and what isn’t is by using binary. 1 being on and 0 being off.
2. We will store all of our maps into a 2D integer array, which is returned to the user at the end of each function \(except for when we render\).
3. I will use the array function [GetUpperBound\(\)](https://msdn.microsoft.com/en-us/library/system.array.getupperbound%28v=vs.110%29.aspx) to get the height and width of each map so that we have fewer variables going into each function, and cleaner code.
4. I often use [Mathf.FloorToInt\(\)](https://docs.unity3d.com/ScriptReference/Mathf.FloorToInt.html), this is because the Tilemap coordinate system starts at the bottom left and using Mathf.FloorToInt\(\) allows us to round the numbers to an integer.
5. All of the code provided in this blog post is in C\#.

읽기 전 알아두면 좋은점들 :

1. Tile과 아닌것을 구분하는 방법은 binary를 사용하여 아닌것은 0, tile인 것은 1로 표시합니다.
2. 모든 맵을 2D형 int배열에 저장합니다. 이 배열은 함수의 가장 끝에서 사용자에게 반환됩니다.
3. GetUpperBound\(\)함수를 사용하여 각 맵의 높이와 너비를 가져옵니다. 각 기능에 들어갈                           변수들이 줄어듭니다.
4. 작성자는 종종 Mathf,FloorToInt\(\)함수를 사용합니다. Tilemap 좌표시스템이 왼쪽하단에서 시작되고 해당함수를 사용한다면 int형으로 받기 때문입니다.
5. 모든 코드는 C\#으로 작성됩니다.
{% endtab %}

{% tab title="Generate Array" %}
GenerateArray creates a new int array of the size given to it. We can also say whether the array should be full or empty \(1 or 0\). Here’s the code:

GenerateArray는 주어진 크기의 새로운 int 배열을 만듭니다. 또한 배열이 꽉 찼는 지 아니면 비어 있어야하는지 \(1 또는 0\) 말할 수도 있습니다. 코드는 다음과 같습니다

```text
public static int[] GenerateArray(int width, int height, bool empty) {
    int[] map = new int[width, height];
    for (int x = 0; x < map.GetUpperBound(0); x ++) {
        for (int y = 0; y < map.GetUpperBound(1); y ++) {
            if (empty) {
                map[x, y] = 0;
            } else {
                map[x, y] = 1;
            }
        }
    }
    return map;
}
```

> public static으로 2차원 배열의 함수를 선언하고 각각 parameter로                                                           행\(width, 폭 or 넓이\), 열\(height, 길이\), empty 여부를 넘기고, C\#의 API인 Array.GetUpperBound\(\)함수로 배열의 가장 끝 요소를 반환 받습니다. 그리고 반복문을 통해 width, height를 탐색하여 empty 여부로 0, 1을 판별하고 map을 리턴 받습니다.
>
> 여기서 empty가 필요한 이유는 후에 기술하겠습니다.
{% endtab %}

{% tab title="Render Map" %}
This function is used to render our map to the tilemap. We cycle through the width and height of the map, only placing tiles if the array has a 1 at the location we are checking.

이 함수는 맵을 타일 맵에 렌더링하는 데 사용됩니다. 우리는지도의 너비와 높이를 순환하면서 배열이 우리가 확인하는 위치에 1이있는 경우에만 타일을 배치합니다.

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

 Generate Array를 통하여 Map을 empty의 여부에 따라 판별 후 Map을 실질적으로 그리기 위한 함수입니다. UnityEngine.Tilemaps; 을 사용하여 Tilemap에 대한 Component에 접근할 수 있습니다.

{% embed url="https://docs.unity3d.com/ScriptReference/Tilemaps.Tilemap.html" caption="Tilemap Document" %}

Tilemaps namespace에서 쓸 것은 Tilemap 자체의 Component, TileBase Component 두가지를 이용하여 Render Map Method를 구성합니다.

{% hint style="info" %}
Tilemap tilemap : Tilemap Component 자체를 가져와 조작합니다.

TileBase tile : Tilemap의 요소에 tile을 배치하기 위해서 사용합니다.

tilemap parameter를 이용하여 SetTile을 통해 위치 지정 및 tile을 배치하는데 이때 필요한 것이 TileBase tile입니다.
{% endhint %}
{% endtab %}

{% tab title="Update Map" %}
This function is used only to update the map, rather than rendering again. This way we can use less resources as we aren’t redrawing every single tile and its tile data.

이 함수는 다시 렌더링하는 대신 맵을 업데이트하는 데만 사용됩니다. 이렇게하면 **모든 단일 타일과 타일 데이터를 다시 그릴 필요가 없으므로 더 적은 리소스를 사용할 수 있습니다.**

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

중간의 조건문을 통하여 Tile이 비어있는지 아닌지를 검사하여 비어 있다면 SetTile을 하여 위에 굵은 글씨로 표시한 문장이 적용됩니다.
{% endtab %}

{% tab title="Perlin Noise" %}
Perlin Noise란?
{% endtab %}
{% endtabs %}





