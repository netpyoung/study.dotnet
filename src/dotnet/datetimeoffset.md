DateTimeOffset은 .NET Framework 2.0에 도입

예를 들어, 서로 다른 시간대에 있는 서버와 클라이언트 간의 시간을 일관성 있게 유지하려면 단순한 시간 정보만으로는 충분하지 않았습니다.


서버와 클라이언트가 서로 다른 시간대를 사용하는 환경에서 정확한 시간 비교를 가능하게 합니다.


DateTime: 순수한 시간 정보만을 표현하며, 시간대에 대한 정보가 없습니다.
    단순히 로컬 시간대에서 동작하도록 설계되었고
DateTimeOffset: 시간 정보에 더해 UTC와의 오프셋을 명시적으로 포함하며, 시간대를 고려한 비교 및 변환이 가능합니다.


DateTimeOffset은 DateTime과 시간대 오프셋으로 구성된 값 유형입니다,
    Int16 internally to save space, but presented as a TimeSpan


long now = DateTimeOffset.UtcNow.ToUnixTimeSeconds();

https://learn.microsoft.com/en-us/archive/blogs/bclteam/a-brief-history-of-datetime-anthony-moore
https://learn.microsoft.com/en-us/dotnet/standard/datetime/choosing-between-datetime


https://learn.microsoft.com/en-us/previous-versions/windows/embedded/ms912391(v=winembedded.11)

TimeZoneInfo
Calendar	날짜 및 시간 계산을 위한 기본 클래스
DateTimeKind	DateTime의 시간대를 나타내는 열거형

TimeSpan
    public double TotalSeconds { get; }
public static long DaysToSeconds(int days)
{
    TimeSpan timeSpan = TimeSpan.FromDays(days);
    return (long)timeSpan.TotalSeconds;
}


public static long DateTimeToUnixTimeSeconds(DateTime dateTime)
{
    // UNIX 타임스탬프를 계산하기 위해 1970년 1월 1일 00:00:00 UTC로부터의 초를 반환합니다.
    DateTime unixEpoch = new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc);
    return (long)(dateTime - unixEpoch).TotalSeconds;
}


UTC
"Coordinated Universal Time"은 영어 표현입니다.
하지만, 프랑스어에서 "Temps Universel Coordonné"라는 표현도 사용됩니다.

UTC는 전 세계의 표준 시간이며, UNIX 시간은 이 UTC를 기준으로 한 초 단위의 경과 시간입니다

정의: UTC는 전 세계의 표준 시간으로, 지구의 회전과 원자시계를 기준으로 한 시간입니다.
정의: UNIX 시간은 1970년 1월 1일 00:00:00 UTC부터의 경과된 초를 나타냅니다.
