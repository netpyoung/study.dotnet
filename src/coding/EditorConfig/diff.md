``` cs
public enum E_STATE
{
   NONE = 0,
   A,
   B,
}

[SerializeField]
GameObject _prefab

#region Unity callbacks
#endregion // Unity callbacks
```

``` cs
// enum prefix
E_

// private member prefix
_

// bool prefix forcing to using `is` (no should/has/can/must/able ...)
private bool _isSuccess;
public bool IsSuccess {get; set;}


// interface prefix
IAnimal

// abstract prefix
AAnimal

// naming of method for return null
Student GetStudent();       // impossible return null
Student GetStudentOrNull(); // possible return null

// async postfix
Task<Student> GetStudentAsync();

// force Allman style with force bracket
if (a == b)
{
   return a;
}
return b;

// No var: it makes hard to code review especially on the web.

// No query expression. Using Method chain
IEnumerable<Property> properties = source.Where(condition).Select(x => x.Property);    // yes
IEnumerable<Property> properties = from x in source where condition select x.Property; // no

```
