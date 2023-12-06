refer stateless code over stateful code. States are complexity. Making code stateful adds artificial complexities not inherited from the business process. Instead, they are complexities that arise from inappropriate code patterns.

The following are the coding patterns that lead to stateful code, which should be avoided. Below are examples of such patterns and how to avoid them. The patterns to avoid are marked by the comment `To avoid` and the better alternatives are marked by the comment `To write`

```
//To avoid
public class A 
{
   private string _b;
   public void Fee(string b){ _b = b}
}
//To write
public class A 
{
   public string B{get; init;}
}
//To avoid
int i = 0;
...
if(condition) i = 1;
...
//To write
int i = condition ? 1 : 0;

//To avoid 
public int Foo()
{
		var a = repository1.Read();
    a = a++;
    var b = repository2.Read();
    var c = a + b;
    return c;
}
//To write
public static int Foo(int a, int b)
{
   return ++a + b;
}

//To avoid 
public int Fee(IEnumerable<int> numbers)
{
    var result = new List<int>();
    foreach(var num in numbers)
    {
        if(condition) result.add(num);
    }
    return result;
}
//To write 
public int Fee(IEnumerable<int> numbers)
{
    return numbers.where(condition);
}
```
