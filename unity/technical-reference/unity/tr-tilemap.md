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
### Returns

**float** Value between 0.0 and 1.0. \(Return value might be slightly beyond 1.0.\)

float 0.0과 1.0 사이의 값. \(반환 값은 1.0을 약간 초과 할 수 있습니다.\)

### Description

Perlin noise is a pseudo-random pattern of float values generated across a 2D plane \(although the technique does generalise to three or more dimensions, this is not implemented in Unity\). The noise does not contain a completely random value at each point but rather consists of "waves" whose values gradually increase and decrease across the pattern. The noise can be used as the basis for texture effects but also for animation, generating terrain heightmaps and many other things.

Perlin 노이즈는 2D 평면에서 생성 된 float 값의 pseduo random pattern\(의사 랜덤 패턴\)입니다.      \(기술은 3 차원 이상으로 일반화되지만 Unity에서는 구현되지 않습니다\).                                               노이즈는 각 지점에서 완전히 임의의 값을 포함하지 않고 패턴 전체에서 값이 점차 증가하고 감소하는 "파동"으로 구성됩니다. 노이즈는 텍스처 효과의 기초로 사용될뿐만 아니라 애니메이션, 지형 하이트 맵 및 기타 여러 가지를 생성하는 데 사용될 수 있습니다.

Any point in the plane can be sampled by passing the appropriate X and Y coordinates. The same coordinates will always return the same sample value but the plane is essentially infinite so it is easy to avoid repetition by choosing a random area to sample from.

적절한 X 및 Y 좌표를 전달하여 평면의 모든 점을 샘플링 할 수 있습니다. 동일한 좌표는 항상 동일한 샘플 값을 반환하지만 평면은 본질적으로 무한하므로 샘플링 할 임의의 영역을 선택하여 반복을 피하기 쉽습니다.
{% endtab %}

{% tab title="Mathf.FloorToInt\(\)" %}
Returns the largest integer smaller to or equal to `f`.

f보다 작거나 같은 가장 큰 정수를 반환합니다.

{% embed url="https://docs.unity3d.com/ScriptReference/Mathf.FloorToInt.html" caption="Mathf.FloorToInt\(\) Document" %}

{% embed url="https://m.blog.naver.com/yoohee2018/220692802850" caption="FloorToInt\(\)의 정의 및 Mathf를 사용한 Object Movement에 유용한 함수들" %}
{% endtab %}
{% endtabs %}





