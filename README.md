# Horse-Query

![](https://img.shields.io/github/stars/dliocode/horse-query.svg) ![](https://img.shields.io/github/forks/dliocode/horse-query.svg) ![](https://img.shields.io/github/tag/dliocode/horse-query.svg) ![](https://img.shields.io/github/release/dliocode/horse-query.svg) ![](https://img.shields.io/github/issues/dliocode/horse-query.svg)

Support: developer.dlio@gmail.com

Middleware for Horse. Use to send a dataset in json format.

### For install in your project using [boss](https://github.com/HashLoad/boss):
``` sh
$ boss install github.com/dliocode/horse-query
```

## Usage

```delphi
uses Horse, Horse.Query;

begin
  THorse.Use(Query);

  THorse.Get('/ping',
    procedure(Req: THorseRequest; Res: THorseResponse; Next: TProc)
    var
      Memtable: TFDMemtable;
    begin
      Memtable := TFDMemtable.Create(nil);
      Memtable.FieldDefs.Add('Codigo', ftInteger, 0, False);
      Memtable.FieldDefs.Add('Nome', ftString, 100, False);

      Memtable.LogChanges := False;
      Memtable.CachedUpdates := True;

      Memtable.Close;
      Memtable.CreateDataSet;
      Memtable.Open;

      Memtable.Append;
      Memtable.FieldByName('Codigo').AsInteger := 1;
      Memtable.FieldByName('Nome').AsString := 'Ping';
      Memtable.Post;

      Memtable.Append;
      Memtable.FieldByName('Codigo').AsInteger := 2;
      Memtable.FieldByName('Nome').AsString := 'Pong';
      Memtable.Post;

      Memtable.ApplyUpdates;

      Res.Send<TFDMemtable>(Memtable);
    end);

  THorse.Listen(9002,
    procedure(Horse: THorse)
    begin
      Writeln(Format('Server started in %s:%d', [THorse.Host, THorse.Port]));
      Writeln('Press return to stop...');
    end);
end.
```

## License

MIT © [Danilo Lucas](https://github.com/dliocode/)