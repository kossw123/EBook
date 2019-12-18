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

{% tab title="" %}

{% endtab %}
{% endtabs %}

