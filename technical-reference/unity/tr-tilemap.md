---
description: Tilemap Technical reference
---

# TR Tilemap

## Procedural Pattern Tilemap

이 단락에서는 아래와 같은 기술문서를 다루고 있습니다.

* Array.GetUpperBound\(int dimension\)

{% tabs %}
{% tab title="Array.GetUpperBound\(int dimension\)" %}
배열에서 지정된 차원의 마지막 요소의 인덱스를 가져옵니다.

즉, 배열의 가장 끝 주소의 내용을 반환합니다. 위의 내용을 1차원 배열에만 적용합니다.                  Procedural Pattern Tilemap에서는 2차원 배열을 쓰기 때문에 위치가 \( 0, 0 \)부터 시작한다면 GetUpperBound\(\)함수를 이용해 각각 width, height의 가장 큰 요소를 반환합니다.

여기서 요소라 함은 

|  | Column 1 | Column 2 | Column 3 | Column 4 | Column 5 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Row 1 | 0 , 0 | 0 , 1 | 0, 2 | 0, 3 | 0, 4 |
| Row 2 | 1, 0 | 1, 1 | 1, 2 | 1, 3 | 1, 4 |
| Row 3 | 2, 0 | 2, 1 | 2, 2 | 2, 3 | 2, 4 |
| Row 4 | 3, 0 | 3, 1 | 3, 2 | 3, 3 | 3, 4 |

위의 도표는 임의로 2차원 배열의 5\*4행렬을 배치 한 것인데, 각 칸마다 들어가 있는 번호는 위치를 의미합니다. 여기서 GetUpperBound\(0\)을 한다면 1차원에 있는 배열중 가장 큰 요소를 반환하기 때문에 4를 반환합니다. 마찬가지로 GetUpperBound\(1\)을 한다면 2차원에 있는 배열중 가장 큰요소를 반환하기 때문에 5를 반환합니다.

{% embed url="https://docs.microsoft.com/ko-kr/dotnet/api/system.array.getupperbound?view=netframework-4.8" caption="Array.GetUpperBound, GetLowerBound 예시" %}

 위의 함수를 이용해 Map의 가로,세로의 끝을 조사하여 반복문을 돌려 해당 위치에 Tile이 없다고 할 때 0, 있다면 1로 표시합니다.
{% endtab %}
{% endtabs %}

## Render Map

이 단락에서는 아래와 같은 기술문서를 다루고 있습니다.

* UnityEngine.Tilemaps

{% tabs %}
{% tab title="UnityEngine.Tilemaps" %}
The tile map stores sprites in a layout marked by a Grid component

Tilemap은 Sprite를 Grid Component로 표시된 레이아웃에 저장합니다.

{% embed url="https://docs.unity3d.com/ScriptReference/Tilemaps.Tilemap.html" caption="UnityEngine.Tilemaps Document" %}

{% hint style="info" %}
Render Map에 쓰인 Method

Tilemap tilemap : Tilemap Component 자체를 가져와 조작합니다.

TileBase tile : Tilemap의 요소에 tile을 배치하기 위해서 사용합니다.

tilemap parameter를 이용하여 SetTile을 통해 위치 지정 및 tile을 배치하는데 이때 필요한 것이 TileBase tile입니다.
{% endhint %}
{% endtab %}
{% endtabs %}

## Perlin Noise

이 단락에서는 아래와 같은 기술문서를 다루고 있습니다.

* Mathf.PerlinNoise\(\)
* Mathf.FloorToInt\(\)

{% tabs %}
{% tab title="Mathf.PerlinNoise\(\)" %}

{% endtab %}

{% tab title="Mathf.FloorToInt\(\)" %}
{% embed url="https://docs.unity3d.com/ScriptReference/Mathf.FloorToInt.html" caption="Mathf.FloorToInt\(\) Document" %}

{% embed url="https://m.blog.naver.com/yoohee2018/220692802850" caption="FloorToInt\(\)의 정의 및 Mathf를 사용한 Object Movement에 유용한 함수들" %}
{% endtab %}
{% endtabs %}



