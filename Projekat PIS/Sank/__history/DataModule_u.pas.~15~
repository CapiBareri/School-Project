unit DataModule_u;

interface

uses
  System.SysUtils, System.Classes, FireDAC.Stan.Intf, FireDAC.Stan.Option,
  FireDAC.Stan.Error, FireDAC.UI.Intf, FireDAC.Phys.Intf, FireDAC.Stan.Def,
  FireDAC.Stan.Pool, FireDAC.Stan.Async, FireDAC.Phys, FireDAC.Phys.SQLite,
  FireDAC.Phys.SQLiteDef, FireDAC.Stan.ExprFuncs,
  FireDAC.Phys.SQLiteWrapper.Stat, FireDAC.FMXUI.Wait, FireDAC.Stan.Param,
  FireDAC.DatS, FireDAC.DApt.Intf, FireDAC.DApt, Data.DB, FireDAC.Comp.DataSet,
  FireDAC.Comp.Client, IdBaseComponent, IdComponent, IdCustomTCPServer,
  IdTCPServer, IdContext, FMX.Dialogs;

type
  TDataModule1 = class(TDataModule)
    FDConnection1: TFDConnection;
    FDQuery1: TFDQuery;
    IdTCPServer1: TIdTCPServer;
    procedure IdTCPServer1Execute(AContext: TIdContext);
    procedure DataModuleCreate(Sender: TObject);
  private
    { Private declarations }
  public
    { Public declarations }
  end;

var
  DataModule1: TDataModule1;

implementation

{%CLASSGROUP 'FMX.Controls.TControl'}

{$R *.dfm}

procedure TDataModule1.DataModuleCreate(Sender: TObject);
begin
  IdTCPServer1.DefaultPort := 6000;
  IdTCPServer1.Active := True;
end;

procedure TDataModule1.IdTCPServer1Execute(AContext: TIdContext);
var
  Password: Integer;
  UserID: Integer;
begin
  UserID := -1;

  try
    Password := AContext.Connection.IOHandler.ReadInt32;

    TThread.Synchronize(nil, procedure
    begin
      FDQuery1.Close;
      FDQuery1.SQL.Text := 'SELECT worker_id FROM worker WHERE password = :pwd';
      FDQuery1.ParamByName('pwd').AsInteger := Password;
      FDQuery1.Open;

      if not FDQuery1.IsEmpty then
        UserID := FDQuery1.FieldByName('worker_id').AsInteger;
    end);

    AContext.Connection.IOHandler.Write(Int32(UserID));

  except
    on E: Exception do
    begin
      AContext.Connection.IOHandler.Write(Int32(-1));
    end;
  end;
end;

end.
