---
description: OpenGL
---

# OpenGL



```text
using System; 
using System.Collections.Generic; 
using System.Linq; 
using System.Text; 
using System.Threading.Tasks;
using System.Reflection;

namespace Reflection 
{ 

    class Program { static void Main(string[] args) 
    {
        _Paragraph part = new _Paragraph();
        //part.PropsSetting("Tom", 1, 10.12f);
        part.PropsSetting(null, 0, 0.0f);

        GetPropertyValues(part);

        Console.WriteLine();
        Console.WriteLine();
        Console.WriteLine();
        Console.WriteLine();

        PropsValuesCheck(part);


    }

    private static void GetPropertyValues(Object obj)
    {
        Type t = obj.GetType();
        Console.WriteLine("Type is: {0}", t.Name);
        PropertyInfo[] props = t.GetProperties();
        Console.WriteLine("Properties (N = {0}):",
                          props.Length);
        foreach (var prop in props)
            if (prop.GetIndexParameters().Length == 0)
                Console.WriteLine("   {0} ({1}): {2}", prop.Name,
                                  prop.PropertyType.Name,
                                  prop.GetValue(obj));
            else
                Console.WriteLine("   {0} ({1}): <Indexed>", prop.Name,
                                  prop.PropertyType.Name);
    }

    private static void PropsValuesCheck(Object obj)
    {
        Type t = obj.GetType();
        PropertyInfo[] props = t.GetProperties();

        foreach(PropertyInfo item in props)
        {
            if (item.GetValue(obj) is null || item.GetValue(obj) is 0 || item.GetValue(obj) is 0.0f)
                Console.WriteLine("Null Check!! : " + item.Name);
        }
    }
}


public class _Paragraph
{
    public string name { get; private set; }
    public int id { get; private set; }
    public float density { get; private set; }

    public object temp { get; private set; }

    //public object[] tempArr { get; private set; }

    //public object this[int index]
    //{
    //    get
    //    {
    //        return tempArr[index];
    //    }
    //    set
    //    {
    //        tempArr[index] = value;
    //    }
    //}

    //public _Paragraph()
    //{
    //    tempArr = new object[3];
    //}
    public void PropsSetting(string param1, int param2, float param3)
    {
        this.name = param1;
        this.id = param2;
        this.density = param3;

        //tempArr[0] = param1;
        //tempArr[1] = param2;
        //tempArr[2] = param3;

    }
}
```

}

