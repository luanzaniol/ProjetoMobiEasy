# ProjetoMobiEasy
unit frmprincipal;

interface

uses
  System.SysUtils, System.Types, System.UITypes, System.Classes, System.Variants,
  FMX.Types, FMX.Controls, FMX.Forms, FMX.Graphics, FMX.Dialogs,
  FMX.Controls.Presentation, FMX.MultiView, FMX.Layouts, FMX.StdCtrls,
  System.ImageList, FMX.ImgList, FMX.Objects,FMX.ANI, FMX.TabControl,
  System.Rtti, FMX.Grid.Style, FMX.ScrollBox, FMX.Grid, ClientClassesUnit1,
  ClientModuleUnit1, conf;

type
  TFormMem = class(TForm)
    MultiView1: TMultiView;
    Layout1: TLayout;
    ToolBar1: TToolBar;
    masterbutton: TSpeedButton;
    Relatórios: TSpeedButton;
    btnPrincipal: TSpeedButton;
    ImageList1: TImageList;
    ToolBar2: TToolBar;
    tbMenu: TLabel;
    LayoutCONTEINER: TLayout;
    Splitter1: TSplitter;
    btnClientee: TSpeedButton;
    lbl1: TLabel;
    ToolBar3: TToolBar;
    btn2: TSpeedButton;
    sair: TSpeedButton;
    btnrelatorio: TSpeedButton;
    rctngl1: TRectangle;
    lbl2: TLabel;
    lbl3: TLabel;
    SpeedButton1: TSpeedButton;
    SpeedButton2: TSpeedButton;
    procedure FormCreate(Sender: TObject);
    procedure btnClienteClick(Sender: TObject);
    procedure SpeedButton1Click(Sender: TObject);
    procedure btnClienteeClick(Sender: TObject);
    procedure FormVirtualKeyboardHidden(Sender: TObject;
      KeyboardVisible: Boolean; const Bounds: TRect);
    procedure FormVirtualKeyboardShown(Sender: TObject;
      KeyboardVisible: Boolean; const Bounds: TRect);
    procedure btnPrincipalClick(Sender: TObject);
    procedure RelatóriosClick(Sender: TObject);
    procedure btn1Click(Sender: TObject);
    procedure FormKeyUp(Sender: TObject; var Key: Word; var KeyChar: Char;
      Shift: TShiftState);
    procedure sairClick(Sender: TObject);
    procedure btn2Click(Sender: TObject);

  private
    { Private declarations }
    factiveform : TForm;
    procedure FormOpen(aform : TComponentClass);
    procedure formopen2(aform2 : TComponentClass);
    procedure formopenconf(aformconf : TComponentClass);
  public
    { Public declarations }
  end;

var
  FormMem: TFormMem;
  FTecladoShow: Boolean;

implementation


{$R *.fmx}
{$R *.LgXhdpiPh.fmx ANDROID}

uses vendas, frm3, frminicial;

procedure TFormMem.btn1Click(Sender: TObject);
begin
formopen(TFormMem);    //chamar tela principal
masterbutton.Visible := True;
//MultiView1.MasterButton := btn1;
//btn1.Visible := False;
end;

procedure TFormMem.btn2Click(Sender: TObject);
begin
ClientModuleUnit1.ClientModule1.cds_cliente.Open;
FormOpen2(TForm3);
end;

procedure TFormMem.btnClienteClick(Sender: TObject);
begin
FormOpen2(TForm1);
end;

procedure TFormMem.btnClienteeClick(Sender: TObject);
begin
formopenconf(TForm4);
end;

procedure TFormMem.btnPrincipalClick(Sender: TObject);
begin
formopen(TFormMem);

end;

procedure TFormMem.FormCreate(Sender: TObject);

begin
factiveform := nil;


end;

procedure TFormMem.FormKeyUp(Sender: TObject; var Key: Word; var KeyChar: Char;
  Shift: TShiftState);
begin
if Key = vkHardwareBack then
begin
 formopen(TFormMem);
 //btn1.Visible := False;
 masterbutton.Visible := True;
  key := 0;
end;





 end;





procedure TFormMem.FormOpen(aform: TComponentClass);
var
  i: Integer;
begin
  if (FActiveForm = nil) or (Assigned(FActiveForm) and
    (FActiveForm.ClassName <> aForm.ClassName)) then
  begin
    for i := LayoutCONTEINER.ControlsCount - 1 downto 0 do
      LayoutCONTEINER.RemoveObject(LayoutCONTEINER.Controls[i]);

    FActiveForm.DisposeOf;
    FActiveForm := nil;

    Application.CreateForm(aForm, FActiveForm);
    LayoutCONTEINER.AddObject(TLayout(FActiveForm.FindComponent('LayoutCONTEINER')));
  end;

  MultiView1.HideMaster;
end;

procedure TFormMem.formopen2(aform2: TComponentClass);
var
  i: Integer;
begin
  if (FActiveForm = nil) or (Assigned(FActiveForm) and
    (FActiveForm.ClassName <> aForm2.ClassName)) then
  begin
    for i := LayoutCONTEINER.ControlsCount - 1 downto 0 do
      LayoutCONTEINER.RemoveObject(LayoutCONTEINER.Controls[i]);

    FActiveForm.DisposeOf;
    FActiveForm := nil;

    Application.CreateForm(aForm2, FActiveForm);
    LayoutCONTEINER.AddObject(TLayout(FActiveForm.FindComponent('LayoutClient')));
  end;

  MultiView1.HideMaster;
end;

procedure TFormMem.formopenconf(aformconf: TComponentClass);
var
  i: Integer;
begin
  if (FActiveForm = nil) or (Assigned(FActiveForm) and
    (FActiveForm.ClassName <> aformconf.ClassName)) then
  begin
    for i := LayoutCONTEINER.ControlsCount - 1 downto 0 do
      LayoutCONTEINER.RemoveObject(LayoutCONTEINER.Controls[i]);

    FActiveForm.DisposeOf;
    FActiveForm := nil;

    Application.CreateForm(aformconf, FActiveForm);
    LayoutCONTEINER.AddObject(TLayout(FActiveForm.FindComponent('LayoutClient')));
  end;

  MultiView1.HideMaster;
end;

procedure TFormMem.FormVirtualKeyboardHidden(Sender: TObject;
  KeyboardVisible: Boolean; const Bounds: TRect);
begin
     FTecladoShow := false;

     if not KeyboardVisible then
        AnimateFloat('Padding.Top', 0, 0.1);
end;

procedure TFormMem.FormVirtualKeyboardShown(Sender: TObject;
  KeyboardVisible: Boolean; const Bounds: TRect);
VAR O:TFMXObject;
begin
   FTecladoShow := true;

     if Assigned(Focused) and (Focused.GetObject is TControl) then
        if TControl(Focused).AbsoluteRect.Bottom - Padding.Top >= (Bounds.Top ) then
        begin
             //If switching between controls, the KeyboardHidden animation will run first
             //and we'll see the form scroll up and then down.
             //Calling StopPropertyAnimation jumps the first animation to it's final value - same problem
             //Instead we need to search for the other animation and call StopAtCurrent.
             for O in Children do
                 if (O is TFloatAnimation) and (TFloatAnimation(O).PropertyName = 'Padding.Top') then
                    TFloatAnimation(O).StopAtCurrent;

             AnimateFloat('Padding.Top',Bounds.Top  - TControl(Focused).AbsoluteRect.Bottom + Padding.Top, 0.1)
        end
        else
     else
        AnimateFloat('Padding.Top', 0, 0.1);
end;

procedure TFormMem.RelatóriosClick(Sender: TObject);
begin
formopen2(TForm1);
end;

procedure TFormMem.sairClick(Sender: TObject);
begin
Application.Terminate;
end;

procedure TFormMem.SpeedButton1Click(Sender: TObject);
begin
FormOpen(TForm1);
end;

end.
