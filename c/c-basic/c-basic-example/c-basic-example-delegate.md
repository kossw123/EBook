---
description: 'C# Basic Example delegate'
---

# C\# Basic Example delegate

## 무엇을 하려고 하는가?

* delegate에 대한 코드를 짜보고, 해당 코드에 대한 리뷰를 합니다.

```csharp
using System;
public class SamplesDelegate  {

   // Declares a delegate for a method that takes in an int and returns a String.
   public delegate String myMethodDelegate( int myInt );

   // Defines some methods to which the delegate can point.
   public class mySampleClass  {

      // Defines an instance method.
      public String myStringMethod ( int myInt )  {
         if ( myInt > 0 )
            return( "positive" );
         if ( myInt < 0 )
            return( "negative" );
         return ( "zero" );
      }

      // Defines a static method.
      public static String mySignMethod ( int myInt )  {
         if ( myInt > 0 )
            return( "+" );
         if ( myInt < 0 )
            return( "-" );
         return ( "" );
      }
   }

   public static void Main()  {

      // Creates one delegate for each method. For the instance method, an
      // instance (mySC) must be supplied. For the static method, use the
      // class name.
      mySampleClass mySC = new mySampleClass();
      myMethodDelegate myD1 = new myMethodDelegate( mySC.myStringMethod );
      myMethodDelegate myD2 = new myMethodDelegate( mySampleClass.mySignMethod );

      // Invokes the delegates.
      Console.WriteLine( "{0} is {1}; use the sign \"{2}\".", 5, myD1( 5 ), myD2( 5 ) );
      Console.WriteLine( "{0} is {1}; use the sign \"{2}\".", -3, myD1( -3 ), myD2( -3 ) );
      Console.WriteLine( "{0} is {1}; use the sign \"{2}\".", 0, myD1( 0 ), myD2( 0 ) );
   }
}


/*
This code produces the following output:

5 is positive; use the sign "+".
-3 is negative; use the sign "-".
0 is zero; use the sign "".
*/
```

위의 코드는 mySampleClass Class에서 선언한 Field의 string type의 멤버 함수들을 myMethodDelegate에 등록하여 동적으로 생성하고 있습니다.

myMethodDelegate에 대한 delegate 객체는 instance Method와 중첩된 static으로 선언된 mySignMethod에 대해 만들어지고 있습니다.

delegate를 동적으로 생성한다는 것에 



{% embed url="https://docs.microsoft.com/ko-kr/dotnet/api/system.delegate?view=netcore-3.1\#examples" %}



