``` cs
public static SmartFormatter CreateDefaultSmartFormat(SmartSettings? settings = null)
{
    // Register all default extensions here:
    var smart = new SmartFormatter(settings)
        // Extension are sorted automatically
        .AddExtensions(
            new StringSource(),
            // will automatically be added to the IFormatter list, too
            new ListFormatter(),
            new DictionarySource(),
            new ValueTupleSource(),
            new ReflectionSource(),
            // for string.Format behavior
            new DefaultSource(),
            new KeyValuePairSource()
        )
        .AddExtensions(
            new PluralLocalizationFormatter(),
            new ConditionalFormatter(),
            new IsMatchFormatter(),
            new NullFormatter(),
            new ChooseFormatter(),
            new SubStringFormatter(),
            // for string.Format behavior
            new DefaultFormatter()
        );

    return smart;
}

// ISource
//   bool TryEvaluateSelector(ISelectorInfo selectorInfo);
// IFormatter
//   string Name { get; set; }
//   bool CanAutoDetect { get; set; }
//   bool TryEvaluateFormat(IFormattingInfo formattingInfo);
```


```
        JsonSerializerOptions options = new JsonSerializerOptions
        {
            //WriteIndented = true,
            IncludeFields = true,
            Encoder = JavaScriptEncoder.Create(new TextEncoderSettings(UnicodeRanges.All)),
            Converters = { new JsonStringEnumConverter() }
        };
```