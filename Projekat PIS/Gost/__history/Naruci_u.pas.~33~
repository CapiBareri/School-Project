unit Naruci_u;

interface

uses
  System.SysUtils, System.Types, System.UITypes, System.Classes, System.Variants,
  FMX.Types, FMX.Controls, FMX.Forms, FMX.Graphics, FMX.Dialogs,
  FMX.Controls.Presentation, FMX.StdCtrls, IdBaseComponent, IdComponent,
  IdTCPConnection, IdTCPClient, System.JSON, FMX.Layouts, FMX.ListBox;

type
  TForm2 = class(TForm)
    btnDomaca: TButton;
    IdTCPClient1: TIdTCPClient;
    listBoxItems: TListBox;
    btnNaruci: TButton;
    procedure FormCreate(Sender: TObject);
    procedure btnDomacaClick(Sender: TObject);
    procedure SendItems;
    procedure AddItemsDisplay;
    procedure btnNaruciClick(Sender: TObject);
    procedure ClearItems;
  private
    FItems: TStringList;
  public

  end;

var
  Form2: TForm2;

implementation

{$R *.fmx}

uses
Gost_u;

procedure TForm2.btnDomacaClick(Sender: TObject);
var
  NewItemName: string;
  NewQty: Integer;
begin
  NewItemName := 'Domaca';
  NewQty := 1;

  FItems.Values[NewItemName] := IntToStr(NewQty);

  AddItemsDisplay;

end;

procedure TForm2.btnNaruciClick(Sender: TObject);
begin
Form1.Show;
Form2.Hide;
SendItems;
end;

procedure TForm2.FormCreate(Sender: TObject);
begin
  IdTCPClient1.Connect;
  FItems := TStringList.Create;
  FItems.NameValueSeparator := ',';
end;

procedure TForm2.AddItemsDisplay;
var
i: Integer;
begin
for i := 0 to FItems.Count - 1 do
    listBoxItems.Items.Add(FItems.Names[i] + ': ' + FItems.ValueFromIndex[i])
end;

procedure TForm2.SendItems;
var
  JSONObject: TJSONObject;
  ItemsArray: TJSONArray;
  ItemObj: TJSONObject;
  i: Integer;
begin
  JSONObject := TJSONObject.Create;
  ItemsArray := TJSONArray.Create;
  try
    JSONObject.AddPair('type', 'order');
    for i := 0 to FItems.Count - 1 do
    begin
      ItemObj := TJSONObject.Create;
      ItemObj.AddPair('name', FItems.Names[i]);
      ItemObj.AddPair('quantity', TJSONNumber.Create(StrToInt(FItems.ValueFromIndex[i])));
      ItemsArray.Add(ItemObj);
    end;
    JSONObject.AddPair('items', ItemsArray);

    IdTCPClient1.IOHandler.WriteLn(JSONObject.ToString);
  finally
    JSONObject.Free;
  end;
end;

procedure TForm2.ClearItems;
begin
  FItems.Clear;
  listBoxItems.Clear;
end;

end.
