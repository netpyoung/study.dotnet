## TODO

https://andrewlock.net/updates-to-the-stronglytypedid-library/

https://github.com/praeclarum/sqlite-net/blob/master/src/SQLite.cs


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



- https://www.sysnet.pe.kr/2/1/946?pageno=0
  - Custom LINQ Provider - [1]. 소개 ; https://umjunil.tistory.com/107
  - Custom LINQ Provider - [2]. Custom LINQ Provider 만들기 (IQueryable) ; https://umjunil.tistory.com/108
  - Custom LINQ Provider - [3]. Custom LINQ Provider 만들기 (IQueryProvider) ; https://umjunil.tistory.com/109
  - Custom LINQ Provider - [4]. Query(쿼리)를 이용한 원격 개체 탐색 ; https://umjunil.tistory.com/111
  - Custom LINQ Provider - [5]. LINQ To Naver Open API ; https://umjunil.tistory.com/112


IQueryable<>
IQueryProvider