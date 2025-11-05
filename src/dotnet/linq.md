## TODO

https://andrewlock.net/updates-to-the-stronglytypedid-library/

https://github.com/praeclarum/sqlite-net/blob/master/src/SQLite.cs


## expression tree - expression visitor

https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/expression-trees/expression-trees-interpreting
https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/expression-trees/
https://rafalkozlowski.engineer/how-to-use-expressionvisitor-like-a-pro/

- rowid
  - https://sqlite.org/withoutrowid.html
  - 정수가 아닌 PRIMARY KEY가 있는 테이블에 ROWID 없이 


https://github.com/praeclarum/sqlite-net/issues/1106
https://github.com/DotNetBrightener/LinQ-To-Sql-Builder
https://github.com/DapperLib/Dapper
https://github.com/linq2db/linq2db
 



``` csharp

public interface IQueryable : System.Collections.IEnumerable {}
public interface IQueryProvider {}


var query = db.Table<Stock>().Where(v => v.Symbol.StartsWith("A"));

ISQLiteConnection
    TableQuery<T> Table<T> () where T : new();

public partial class SQLiteConnection : IDisposable, ISQLiteConnection


public TableQuery<T> Table<T> () where T : new()
{
    return new TableQuery<T> (this);
}


public abstract class BaseTableQuery
{
    protected class Ordering
    {
        public string ColumnName { get; set; }
        public bool Ascending { get; set; }
    }
}
public class TableQuery<T> : BaseTableQuery, IEnumerable<T>
    public TableQuery (SQLiteConnection conn)
    {
        Connection = conn;
        Table = Connection.GetMapping (typeof (T));
    }

public TableMapping GetMapping (Type type, CreateFlags createFlags = CreateFlags.None)
{
    TableMapping map;
    var key = type.FullName;
    lock (_mappings) {
        if (_mappings.TryGetValue (key, out map)) {
            if (createFlags != CreateFlags.None && createFlags != map.CreateFlags) {
                map = new TableMapping (type, createFlags);
                _mappings[key] = map;
            }
        }
        else {
            map = new TableMapping (type, createFlags);
            _mappings.Add (key, map);
        }
    }
    return map;
}


public TableMapping (Type type, CreateFlags createFlags = CreateFlags.None)
{
    TableAttribute.Name이 있으면 그걸로 테이블 이름, 없으면 타입이름으로 테이블 이름
}


public TableQuery<T> Where (Expression<Func<T, bool>> predExpr)
{
    if (predExpr.NodeType == ExpressionType.Lambda) {
        var lambda = (LambdaExpression)predExpr;
        var pred = lambda.Body;
        var q = Clone<T> ();
        q.AddWhere (pred);
        return q;
    }
}
```

ref: https://dotnettutorials.net/lesson/differences-between-ienumerable-and-iqueryable/



- Custom LINQ Provider
  - <https://www.sysnet.pe.kr/2/1/946?pageno=0>
  - [Custom LINQ Provider - [1]. 소개](https://umjunil.tistory.com/107)
  - [Custom LINQ Provider - [2]. Custom LINQ Provider 만들기 (IQueryable)](https://umjunil.tistory.com/108)
  - [Custom LINQ Provider - [3]. Custom LINQ Provider 만들기 (IQueryProvider)](https://umjunil.tistory.com/109)
  - [Custom LINQ Provider - [4]. Query(쿼리)를 이용한 원격 개체 탐색](https://umjunil.tistory.com/111)
  - [Custom LINQ Provider - [5]. LINQ To Naver Open API](https://umjunil.tistory.com/112)


IQueryable<>
IQueryProvider





## IQueryable<> : IEnumerable<>

https://docs.microsoft.com/ko-kr/dotnet/api/system.linq.iqueryable-1?view=netcore-3.1

ElementType	 IQueryable의 이 인스턴스에 연결된 식 트리가 실행될 때 반환되는 요소의 형식을 가져옵니다.
Expression	 IQueryable의 인스턴스에 연결된 식 트리를 가져옵니다.
Provider	 이 데이터 원본에 연결 된 쿼리 공급자를 가져옵니다.

GetEnumerator()	

## IOrderedQueryable<T>

- 정렬

## public interface IQueryProvider

METHODS
CreateQuery<TElement>(Expression)	Constructs an IQueryable<T> object that can evaluate the query represented by a specified expression tree.
Execute<TResult>(Expression)	 Executes the strongly-typed query represented by a specified expression tree.


Expression
https://docs.microsoft.com/en-us/dotnet/api/system.linq.expressions.expression?view=netcore-3.1

## IQueryContext


Expression<Func<int,int,int>> f = (v1,v2) => v1 + v2;
BinaryExpression be = (BinaryExpression)f.Body;
v1 v2 Add  == be.Left, be.Right, be.NodeType

Expression<Func<int, bool>> f = (v1) => ( v1 == 5 );
BinaryExpression be = (BinaryExpression)f.Body;
ConstantExpression ce = (ConstantExpression)be.Right;
5== ce.Value


UnaryExpression 

Expression<Func<string,string,bool>> func = (s1,s2) => ( s1.Substring(0,2) == s2 );
BinaryExpression be        = func.Body as BinaryExpression;   // ( s1.Substring(0,2) == s2 )
MethodCallExpression me    = be.Left as MethodCallExpression; // s1.Substring(0,2)
Assert.Equal("Substring", me.Method.Name);                    // Substring

======================================================














public TResult Execute<TResult>(Expression expression)
       {
       var exp             = expression as MethodCallExpression;
       var func     = (exp.Arguments[1] as UnaryExpression).Operand as Expression<Func<Person, bool>>;
       var lambda   = Expression.Lambda<Func<Person, bool>>(func.Body, func.Parameters[0]);
 
       var r = context.DataSource.Where(lambda.Compile());
 
       TranslateExpression trans = new TranslateExpression();
       sbLog.Append( trans.Translate(expression) );
 
       return (TResult)r.GetEnumerator();
}


public class TranslateExpression
{
       private StringBuilder sb   = new StringBuilder();
 
       public string Translate(Expression expression)
       {
             this.Visit(expression);
 
             return sb.ToString();
       }
}

protected Expression Visit(Expression expression)
{
       switch (expression.NodeType)
       {
             case ExpressionType.Call:
                    return this.VisitCall((MethodCallExpression)expression);
 
             case ExpressionType.Constant:
                    return this.VisitConstant((ConstantExpression)expression);
 
             case ExpressionType.Lambda:
                    return this.VisitLambda((LambdaExpression)expression); // => expression
 
             case ExpressionType.MemberAccess:
                    return this.VisitMember((MemberExpression)expression);
 
             default:
                    throw new Exception( string.Format("{0} 은지원하지않습니다", expression.NodeType));
       }
}

protected virtual Expression VisitCall(MethodCallExpression mce)
{
       switch (mce.Method.Name)
       {
             case "Where":
                    sb.Append("SELECT * FROM").Append( Environment.NewLine );
                    this.Visit(mce.Arguments[0]);
                    sb.Append(" AS T WHERE ");
 
                    UnaryExpression ue         = mce.Arguments[1] as UnaryExpression;
                    LambdaExpression le        = ue.Operand as LambdaExpression;
 
 
                    BinaryExpression be        = le.Body as BinaryExpression;
            if (be != null)
                this.VisitBinary(be);
                           break;
 
             case "StartsWith":
                    this.Visit(mce.Object);
                    sb.AppendFormat(" LIKE '{0}%'", mce.Arguments[0].ToString().Replace("\"",""));
                    break;
       }
 
       return mce;
}


protected virtual Expression VisitConstant(ConstantExpression ce)
{
       IQueryable q = ce.Value as IQueryable;
       if (q is IQueryable)
       {
             sb.AppendFormat(" ( SELECT * FROM {0} ) ", q.ElementType.Name)
                    .Append( Environment.NewLine );
       }
       else
       {
             sb.AppendFormat(" {0} ", ce.Value);
       }
 
       return ce;
}

protected virtual Expression VisitMember(MemberExpression me)
{
       sb.Append(me.Member.Name);
 
       return me;
}

protected virtual Expression VisitBinary(BinaryExpression be)
{
       if (be.Left is BinaryExpression)
             this.VisitBinary((BinaryExpression)be.Left);
       else
       {
             this.Visit(be.Left);
       }
 
    switch (be.NodeType)
    {
        case ExpressionType.GreaterThan:
            sb.Append(" > ");
                    break;
 
        case ExpressionType.GreaterThanOrEqual:
                    sb.Append(" >= ");
                    break;
 
             case ExpressionType.LessThan :
                    sb.Append(" < ");
                    break;
 
             case ExpressionType.LessThanOrEqual:
                    sb.Append(" <= ");
                    break;
 
        case ExpressionType.Equal:
            sb.Append(" = ");
                    break;
 
             case ExpressionType.And:
             case ExpressionType.AndAlso:
                    sb.Append(" AND " );
                    break;
 
             case ExpressionType.Or:
                    sb.Append(" OR ");
                    break;
 
             default:
                    throw new Exception( string.Format("{0} 형식은지원하지않습니다", be.NodeType) );
    }
 
       if (be.Right is BinaryExpression)
             this.VisitBinary((BinaryExpression)be.Right);
       else
       {
             this.Visit(be.Right);
       }
 
    return be;
}

public IEnumerator<Person> GetEnumerator()
{
       provider.Execute<IEnumerator<Person>>(this.expression);
 
       SqlConnection cn           = new SqlConnection("Server=xxxx.kr;DataBase=xxxx;UID=xxxx;PWD=xxxx");
       SqlCommand cm              = new SqlCommand( Log, cn);
       cm.CommandType                   = System.Data.CommandType.Text;
 
       cn.Open();
       Console.WriteLine("DataBase Connection!");
 
       SqlDataReader reader       = cm.ExecuteReader();
                   
       List<Person> list          = new List<Person>();
       while (reader.Read())
       {
             list.Add( new Person {
                    Name   = reader["Name"].ToString(),
                    Age    = Convert.ToInt32(reader["Age"])
             });
       }
 
       reader.Close();
       cn.Close();
 
       return list.GetEnumerator();
}

