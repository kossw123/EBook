---
description: Explanation Tilemap
---

# Explanation Tilemap

## 무엇을 하려고 하는가?

* Tilemap에 관한 tutorial을 하면서 쓰였던 Script들의 해설문서를 담고 있습니다.
* 수작업의 최소한을 위해 절차적인 패턴을 기반으로 한 Tilemap 생성(Procedural pattern Tilemap)에 관한 내용이 전반적으로 많기 때문에 이 문서와 동시에 구글링을 통하여 나름대로 이해하는 것이 좋을 수 있습니다.
* 잘못되거나 수정해야할 항목들이 있으면 page guide에 있는 이메일로 지적해 주시면 수정작업에 들어가도록 하겠습니다.



## Scripting Code Explanation

{% tabs %}
{% tab title="What is this blog post about?" %}
We’ll take a look at some of the most common methods of creating a procedural world, and a couple of custom variations that I have created.  Here’s an example of what you may be able to create after reading this article. Three algorithms are working together to create one map, using a [Tilemap](https://docs.unity3d.com/Manual/class-Tilemap.html) and a [RuleTile](https://github.com/Unity-Technologies/2d-extras):

Procedural World를 만들기 위한 일반적인 방법과, 변형적으로 추가한 몇가지 방법들을 소개합니다.다음은 아래의 기사를 읽은 후 만들 수 있는 몇가지 예입니다. 여기서는 Tilemap, Rule Tile을 사용한 세가지의 알고리즘이 하나의 맵을 만듭니다.

When we’re generating a map with any of the algorithms, we will receive an int array which contains all of the new data. We can then take this data and continue to modify it or render it to a tilemap.

알고리즘을 사용하여 맵을 만든다면, 우리는 새로운 데이터가 포함이 된 int형 배열을 받게됩니다.   그런다음 데이터를 가져와서 계속 수정하거나 하나의 Tilemap으로 만들수 있습니다.

Good to know before you read further:

1. The way we distinguish between what’s a tile and what isn’t is by using binary. 1 being on and 0 being off.
2. We will store all of our maps into a 2D integer array, which is returned to the user at the end of each function (except for when we render).
3. I will use the array function [GetUpperBound()](https://msdn.microsoft.com/en-us/library/system.array.getupperbound\(v=vs.110\).aspx) to get the height and width of each map so that we have fewer variables going into each function, and cleaner code.
4. I often use [Mathf.FloorToInt()](https://docs.unity3d.com/ScriptReference/Mathf.FloorToInt.html), this is because the Tilemap coordinate system starts at the bottom left and using Mathf.FloorToInt() allows us to round the numbers to an integer.
5. All of the code provided in this blog post is in C#.

읽기 전 알아두면 좋은점들 :

1. Tile과 아닌것을 구분하는 방법은 binary를 사용하여 아닌것은 0, tile인 것은 1로 표시합니다.
2. 모든 맵을 2D형 int배열에 저장합니다. 이 배열은 함수의 가장 끝에서 사용자에게 반환됩니다.
3. GetUpperBound()함수를 사용하여 각 맵의 높이와 너비를 가져옵니다. 각 기능에 들어갈                           변수들이 줄어듭니다.
4. 작성자는 종종 Mathf,FloorToInt()함수를 사용합니다. Tilemap 좌표시스템이 왼쪽하단에서 시작되고 해당함수를 사용한다면 int형으로 받기 때문입니다.
5. 모든 코드는 C#으로 작성됩니다.
{% endtab %}

{% tab title="Generate Array" %}
GenerateArray creates a new int array of the size given to it. We can also say whether the array should be full or empty (1 or 0). Here’s the code:

GenerateArray는 주어진 크기의 새로운 int 배열을 만듭니다. 또한 배열이 꽉 찼는 지 아니면 비어 있어야하는지 (1 또는 0) 말할 수도 있습니다. 코드는 다음과 같습니다

```
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

> public static으로 2차원 배열의 함수를 선언하고 각각 parameter로                                                           행(width, 폭 or 넓이), 열(height, 길이), empty 여부를 넘기고, C#의 API인 Array.GetUpperBound()함수로 배열의 가장 끝 요소를 반환 받습니다. 그리고 반복문을 통해 width, height를 탐색하여 empty 여부로 0, 1을 판별하고 map을 리턴 받습니다.
>
> 여기서 empty가 필요한 이유는 후에 기술하겠습니다.
{% endtab %}

{% tab title="Render Map" %}
This function is used to render our map to the tilemap. We cycle through the width and height of the map, only placing tiles if the array has a 1 at the location we are checking.

이 함수는 맵을 타일 맵에 렌더링하는 데 사용됩니다. 우리는지도의 너비와 높이를 순환하면서 배열이 우리가 확인하는 위치에 1이있는 경우에만 타일을 배치합니다.

```
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

&#x20;Generate Array를 통하여 Map을 empty의 여부에 따라 판별 후 Map을 실질적으로 그리기 위한 함수입니다. UnityEngine.Tilemaps; 을 사용하여 Tilemap에 대한 Component에 접근할 수 있습니다.

{% embed url="https://docs.unity3d.com/ScriptReference/Tilemaps.Tilemap.html" %}
Tilemap Document
{% endembed %}

Tilemaps namespace에서 쓸 것은 Tilemap 자체의 Component, TileBase Component 두가지를 이용하여 Render Map Method를 구성합니다.
{% endtab %}

{% tab title="Update Map" %}
This function is used only to update the map, rather than rendering again. This way we can use less resources as we aren’t redrawing every single tile and its tile data.

이 함수는 다시 렌더링하는 대신 맵을 업데이트하는 데만 사용됩니다. 이렇게하면 **모든 단일 타일과 타일 데이터를 다시 그릴 필요가 없으므로 더 적은 리소스를 사용할 수 있습니다.**

```
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
This generation takes the simplest form of implementing Perlin Noise into level generation. We can use the Unity function for Perlin Noise to help us, so there is no fancy programming going into it. We are also going to ensure that we have whole numbers for our tilemap by using the function Mathf.FloorToInt().

이 세대는 Perlin Noise를 레벨 생성으로 구현하는 **가장 간단한 형태**를 취합니다. Perlin Noise의 Unity 기능을 사용하면 도움이되므로 멋진 프로그래밍이 필요하지 않습니다. 또한 Mathf.FloorToInt () 함수를 사용하여 타일 맵의 정수를 확보 할 것입니다.



우선 Perlin Noise에 대해서 알아 보겠습니다.

Perlin Noise란?

자연스럽게 정렬된("부드럽게") 의사 난수 시퀀스를 생성하는 알고리즘이라고 설명 할 수 있습니다. 즉, 우리가 쓰는 난수 함수들은 어떤 범위 안에서 무작위로 선택 할 수 있다는 것인데, Perlin Noise를 사용한다면 이를 보다 자연스럽게 선택 할 수 있다는 뜻이 됩니다.

![Khan Academy - Perlin Noise에서 발췌](<../../../../.gitbook/assets/image (25).png>)

오른쪽의 그림은 시간에 따른 순수한 난수를 표시하고, 왼쪽의 그림은 Perlin Noise를 사용한 시간에 따른 난수 그래프 입니다. 확연하게 왼쪽이 좀 더 자연스럽게 표시되고 이는 텍스쳐에 이용이 되어  절차적(어떤 상황을 거치기 위한 단계를 가리키는 말)인 텍스쳐를 표현합니다.

{% hint style="success" %}
Perlin Noise 함수의 원리

Perlin Noise는 보통 난수를 표현하는 것보다 좀 더 자연스럽게 표현이 가능하기 때문에 이를 Tilemap 배치의 알고리즘으로 사용하여 자연스러운 지형지물을 표시합니다. Perlin Noise라는 개념을 설명하기 위해서는 몇가지 알아 둬야 할점이 있습니다.

1. 진폭(amplitude) : 함수가 가질수 있는 최소, 최대 값의 차이
2. 파장(waveLength) : 같은 위상을 가진 서로 이웃한 두 점 사이의 거리,                                                        혹은 한 주기 동안 파동이 진행한 거리
3. 주파수(frequency) : 1/waveLength로 단위 시간동안 어떤 현상이 일어난 횟수
4. 옥타브(octave) : 각각의 더해지는 연속적인 Noise함수
5. 지속성(Persistence) : 진폭의 변화 정도

위의 다섯가지 용어는 아래에서 첨부할 링크에서 알아두면 좋은 용어들입니다.&#x20;



Perlin Noise의 과정

1. Noise 입히고자 하는 평면을 Grid 시키고 각 꼭지점에 랜덤한 Vector를 부여합니다.  1. 이때 2D에서는 x,y가 모두 정수가 아닌 경우 항상 주변의 가까운 정수 좌표로 이루어진 Grid안에 위치하는데, Unity Grid Component를 쓰기 때문에 이러한 걱정은 할 필요가 없습니다.                                                                                                                    2. 각각의 점에 랜덤한 Vector를 부여한다는데 여기서는 map\[ , ]을 통하여 맵을 생성했기 때문에 x축을 반복문을 돌면서 구하면 됩니다.
2. 임의의 점(x, y)를 둘러싼 Grid의 꼭지점에 gradient Vector를 생성합니다.                                                                      1.이때 gradient라는 임의의 방향을 가진 크기가 1인 Vector로 지칭합니다.                                                 2.이 Vector는 pseudo random이라고 할 수 있는데 각 정수 좌표에 매번 동일한 Vector를 부여하기 때문입니다.
3. 임의의 좌표 (x, y)에서 Grid의 각 모서리 점의 위치를 빼서 distance Vector를 만듭니다. 즉, grid 꼭지점에서 점(x, y)로 향하는 Vector를 생성한다.
4. gradient Vector와 거리 Vector 사이의 내적을 구합니다.                                                                              1. 내적해서 나온 결과가 즉, 최종적으로 나오는 결과값에 영향을 줍니다.                           2. 두 벡터들의 크기를 곱한 두 벡터의 내적은 두 벡터 사이의 cos함수와 같습니다.                                                     3. 이로 인해 Vector가 같은방향을 가리키면 양수, 반대 방향이면 음수를 가지게 되고, 수직이면 0입니다. 이 방식을 통해 gradient Vector의 값에 적용시켜서 방향을 가지게 합니다.
5. 4개의 값 사이를 일종의 가중 평균을 통해 보간합니다.
   1. 여기서 보간에 관한 설명은 부족하기 때문에 추후 추가하도록 하겠습니다.
6. 여러 Noise함수의 값이 생성 되면 그 값을 합하면 Octave를 얻게 되고 Perlin Noise가 완성됩니다.
{% endhint %}

&#x20;\- 참고자료

{% embed url="https://m.blog.naver.com/PostView.nhn?blogId=dj3630&logNo=221512874599&categoryNo=0&proxyReferer=https%3A%2F%2Fwww.google.com%2F" %}

{% embed url="https://mzucker.github.io/html/perlin-noise-math-faq.html#toc-about" %}

{% embed url="https://3dmpengines.tistory.com/170" %}
Perlin Noise 참고자료
{% endembed %}



코드를 보면서 나름 나름대로 해석한 것에 대해 설명하겠습니다.

Perlin Noise는 자연스러운 난수 생성을 위한 알고리즘으로써 본 강의에는 Mathf.PerlinNoise()함수를 사용합니다.

1. map.x값의 길이를 받아와 반복문을 돌립니다.
2. Mathf.FloorToInt()함수를 사용하여 Mathf.PerlinNoise() - reduction값을 적거나 같게 받습니다.그 후 map.GetUpperBound(1)을 통해 2차원의 가장 큰 요소를 곱하여 새로운 newPoint를 생성합니다.
3. newPoint에 (map.y의 길이 / 2)를 통하여 가산연산을 합니다.
4. 다시 반복문을 돌리는데 이때 반복변수를 newPoint로 지정하고 0과 같아질 때 까지 감산반복   합니다.
5. x, y = 1로 설정하여 Tile을 채워 넣습니다.
6. map을 반환받아서 LevelGenerator에서 switch문으로 Method들을 호출합니다.

{% hint style="info" %}
MathfFloorToInt()함수를 사용하여 Mathf.PerlinNoise(x, seed) - reduction의 값을 적거나 같은 값으로 반환하는데 이 좀 더 자연스러운 패턴을 위해 생성과정에서 임의로 넣은 값입니다. 값이 낮아 질수록 뭉특하게 생성됩니다.
{% endhint %}

```
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
```
{% endtab %}
{% endtabs %}



















