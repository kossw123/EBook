# Chapter 3. Shader



VBO, VAO, EBO를 통해 Vertex, Fragment Shader를 작성 했으니 이제는 Shader에 대해 좀 더 알아보자.

## Shader의 GLSL이란?

* GL Shader Language의 준말로 C언어와 비슷하다.
* OpenGL에서 그래픽스 파이프라인을 직접 제어할 수 있도록 하기 위해 작성된 언어

## GLSL은 어떻게 사용하는가?

일단 양식이 정해져 있다.

```c
	#version version_number
	in type in_variable_name;
	in type in_variable_name;

	out type out_variable_name;
	  
	uniform type uniform_name;
	  
	void main()
	{
	  // 입력 값을 처리하고 그래픽 작업을 합니다.
	  ...
	  // 처리된 것을 출력 변수로 출력합니다.
	  out_variable_name = weird_stuff_we_processed;
	}
```

1. \#version version\_number : Version 선언
2. in type in\_variable name : in\(입력변수\), out\(출력변수\) Qualifier는 Vertex Input을 정의하고 출력
3. uniform type uniform\_name : CPU에서 GPU로 넘기는 방법중 하나로써, static과 비슷한 역할을 한다.
4. main\(\) {} : 입력값을 처리하고, 그래픽 작업을 하거나, 처리된 것을 출력변수로 출

그리고 In\(입력 변수\), out\(출력변수\)를 통틀어서 Vertex Attribute라고 한다.

{% hint style="info" %}
그래픽 하드웨어마다 Vertex Attribute를 선언할 수 있는 최대치가 정해져 있다.  
그래서 아래의 코드로 꼭 확인하고 진행하도록 하자.
{% endhint %}

```c
	int nrAttributes;
	glGetIntegerv(GL_MAX_VERTEX_ATTRIBS, &nrAttributes);
	std::cout << "Maximum nr of vertex attributes supported: " 
						<< nrAttributes 
						<< std::endl;
```

## GLSL의 양식에 따른 단계 구분

위의 GLSL의 양식에 따라 단계를 구별해보면 다음과 같다.

### 1. Version 정보를 CPU에게 알리는데, OpenGL의 어떤 버전과 어떤 모드를 사용하겠다고 알린다.

![OpenGL, GLSL, Shader&#xC5D0;&#xC11C; &#xC120;&#xC5B8;&#xD560; Version&#xC758; &#xC774;&#xB984;](../../../.gitbook/assets/image%20%28282%29.png)

이전 문서에서 다음과 같이 Vertex Shader를 선언하고 사용했다.

```c
#version 330 core
layout (location = 0) in vec3 aPos;

void main()
{
    gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);
}
```

여기서 \#version 330 core를 사용한다는 것은

* OpenGL의 Version이 3.3이고
* GLSL의 Version이 3.30.6를 사용하고,
* core모드를 사용하여 직접적으로 파이프라인에 개입한다는 의미이다.



### 2. Vertex Attribute에 다른 언어와 같이 Binding할 수 있는데, 조금 특별하게 사용할 수 있다.

앞서 그래픽 하드웨어마다 Vertex Attribute의 갯수가 정해졌다고 했는데,   
앞서 작성했던 Vertex Shader에서 아래의 코드가 정해져 있는 갯수에 대한 자리를 정해주는 역할을 한다.

```c
	layout (location = 0) in vec3 aPos;
```

위 코드를 해석해보자면

* 한정된 Vertex Attribute의 0번에
* 입력변수인 vec3 type의 aPos라는 변수를 위치시킨다.

변수에 대한 Binding인데, Shader에서 사용하는 변수는 다음과 같다.

![](../../../.gitbook/assets/image%20%28283%29.png)

Vector3 변수를 선언한 것과 진배 없지만,   
Core mode이기 때문에 Parse가 아닌 각 타입마다 vector를 사용한다.

각 요소에 Dot\(.\)을 사용하여 접근할 수 있으며, operator 연산도 가능하다.

### \*\* Swizzling이라는 기능

Core mode이기 때문에 type에 까다롭지만, 반대로 각 요소에 대한 연산은 꽤나 자유로운 편이다.  
다음과 같은 예시를 통해 Swizzling이라는 기능을 알아보자.

![](../../../.gitbook/assets/image%20%28289%29.png)

