SOURCE CODE:

function varargout = PROJECTUTS(varargin)
%PROJECTUTS M-file for PROJECTUTS.fig
%      PROJECTUTS, by itself, creates a new PROJECTUTS or raises the existing
%      singleton*.
%
%      H = PROJECTUTS returns the handle to a new PROJECTUTS or the handle to
%      the existing singleton*.
%
%      PROJECTUTS('Property','Value',...) creates a new PROJECTUTS using the
%      given property value pairs. Unrecognized properties are passed via
%      varargin to PROJECTUTS_OpeningFcn.  This calling syntax produces a
%      warning when there is an existing singleton*.
%
%      PROJECTUTS('CALLBACK') and PROJECTUTS('CALLBACK',hObject,...) call the
%      local function named CALLBACK in PROJECTUTS.M with the given input
%      arguments.
%
%      *See GUI Options on GUIDE's Tools menu.  Choose "GUI allows only one
%      instance to run (singleton)".
%
% See also: GUIDE, GUIDATA, GUIHANDLES

% Edit the above text to modify the response to help PROJECTUTS

% Last Modified by GUIDE v2.5 08-May-2021 01:34:43

% Begin initialization code - DO NOT EDIT
gui_Singleton = 1;
gui_State = struct('gui_Name',       mfilename, ...
                   'gui_Singleton',  gui_Singleton, ...
                   'gui_OpeningFcn', @PROJECTUTS_OpeningFcn, ...
                   'gui_OutputFcn',  @PROJECTUTS_OutputFcn, ...
                   'gui_LayoutFcn',  [], ...
                   'gui_Callback',   []);
if nargin && ischar(varargin{1})
   gui_State.gui_Callback = str2func(varargin{1});
end

if nargout
    [varargout{1:nargout}] = gui_mainfcn(gui_State, varargin{:});
else
    gui_mainfcn(gui_State, varargin{:});
end
% End initialization code - DO NOT EDIT


% --- Executes just before PROJECTUTS is made visible.
function PROJECTUTS_OpeningFcn(hObject, eventdata, handles, varargin)
% This function has no output args, see OutputFcn.
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
% varargin   unrecognized PropertyName/PropertyValue pairs from the
%            command line (see VARARGIN)

% Choose default command line output for PROJECTUTS
handles.output = hObject;

% Update handles structure
guidata(hObject, handles);

% UIWAIT makes PROJECTUTS wait for user response (see UIRESUME)
% uiwait(handles.figure1);


% --- Outputs from this function are returned to the command line.
function varargout = PROJECTUTS_OutputFcn(hObject, eventdata, handles)
% varargout  cell array for returning output args (see VARARGOUT);
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Get default command line output from handles structure
varargout{1} = handles.output;


% --- Executes on button press in btnopen.
function btnopen_Callback(hObject, eventdata, handles)
% hObject    handle to btnopen (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

appcitra = guidata(gcbo);
[namafile, formatfile]=uigetfile({'*.jpg';'*.png';'*.bmp'},'tampil gambar');

image=imread(namafile);
guidata(hObject, handles);
axes(handles.citra_asli);
imshow (image), title('Citra Asli');

set(appcitra.figure1, 'userdata', image);
set(appcitra.citra_asli, 'userdata', image);

% --- Executes on button press in btnproses.
function btnproses_Callback(hObject, eventdata, handles)
% hObject    handle to btnproses (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
appcitra=guidata(gcbo);
I = get(appcitra.citra_asli, 'userdata');
if isequal(I,[])
    msgbox('Enter Picture !','Warning','Warm');
else
    %TrueColor%
    negative = 255-1 -I;
    set(appcitra.figure1, 'CurrentAxes',appcitra.axes2_hasil);
    set(imshow(negative));
    imshow(negative),title('Citra NEGATIVE');
    
    %Histogram Negative%
    axes(handles.axes2_histo)
    x=1:256;
    plot(x,imhist(negative(:,:,1)), 'r-');
    hold on;
    plot(x, imhist(negative(:,:,2)), 'g-');
    plot(x, imhist(negative(:,:,3)), 'b-');
    title('Histogram Negative')
    %Citra Biner%
    binary=im2bw(I);
    set(appcitra.figure1, 'CurrentAxes', appcitra.axes1_hasil);
    set(imshow(binary));
    imshow(binary), title('Citra Biner');
    
    %Histo Biner%
    axes(handles.axes1_histo);
    imhist(binary),title('Histogram Biner');
    
    %Citra Grayscale%
    grayscale=rgb2gray(I);
    set(appcitra.figure1, 'CurrentAxes', appcitra.axes3_hasil);
    set(imshow(grayscale));
    imshow(grayscale), title('Citra Grayscale');
    
    %Histo Grayscale%
    axes(handles.axes3_histo);
    imhist(grayscale),title('Histogram Grayscale');
    
    set(appcitra.axes2_hasil, 'userdata', negative);
    set(appcitra.axes1_hasil, 'userdata', binary);
    set(appcitra.axes3_hasil, 'userdata', grayscale);
    
end
    
