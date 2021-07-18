# UAS_PENGOLAHAN_CITRA
Laporan UAS

[filename,pathname] = uigetfile({'.'});
 
    if ~isequal(filename,0)
        axes(handles.axes1)
        cla reset
        set(gca,'XTick',[])
        set(gca,'YTick',[])
 
        axes(handles.axes2)
        cla reset
        set(gca,'XTick',[])
        set(gca,'YTick',[])
 
        axes(handles.axes3)
        cla reset
        set(gca,'XTick',[])
        set(gca,'YTick',[])
 
        set(handles.edit1,'String','')
        set(handles.edit2,'String','')
        set(handles.edit3,'String','')
        set(handles.edit4,'String','')
        set(handles.edit5,'String','')
        set(handles.edit6,'String','')
 
        Img = imread(fullfile(pathname,filename));
        [,,dim] = size(Img);
        if dim == 3
            Img = rgb2gray(Img);
        end
 
        axes(handles.axes1)
        imshow(Img)
        title('Citra Grayscale')
        handles.Img = Img;
        guidata(hObject, handles)
    else
        return
    end
catch
end


% --- Executes on selection change in popupmenu1.
function popupmenu1_Callback(hObject, eventdata, handles)
% hObject    handle to popupmenu1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: contents = cellstr(get(hObject,'String')) returns popupmenu1 contents as cell array
%        contents{get(hObject,'Value')} returns selected item from popupmenu1


% --- Executes during object creation, after setting all properties.
function popupmenu1_CreateFcn(hObject, eventdata, handles)
% hObject    handle to popupmenu1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: popupmenu controls usually have a white background on Windows.
%       See ISPC and COMPUTER.
if ispc && isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor','white');
end


% --- Executes on selection change in popupmenu2.
function popupmenu2_Callback(hObject, eventdata, handles)
% hObject    handle to popupmenu2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: contents = cellstr(get(hObject,'String')) returns popupmenu2 contents as cell array
%        contents{get(hObject,'Value')} returns selected item from popupmenu2


% --- Executes during object creation, after setting all properties.
function popupmenu2_CreateFcn(hObject, eventdata, handles)
% hObject    handle to popupmenu2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: popupmenu controls usually have a white background on Windows.
%       See ISPC and COMPUTER.
if ispc && isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor','white');
end


% --- Executes on button press in pushbutton2.
function pushbutton2_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
try
    Img = handles.Img;
    [row,col,~] = size(Img);
 
    val1 = get(handles.popupmenu1,'Value');
 
    switch val1
        case 1
            Img_noise = imnoise(Img,'salt & pepper',0.2);
        case 2
            Img_noise = uint8(double(Img)+60*rand(row,col));
        case 3
            Img_noise = uint8(double(Img)+10*randn(row,col));
        case 4
            Img_noise = uint8(double(Img)+raylrnd(20,row,col));
    end
 
    axes(handles.axes2)
    imshow(Img_noise)
    title('Citra Terkontaminasi Noise')
 
    MSE = sum(sum((Img-Img_noise).^2))/(row*col);
    RMSE = sqrt(MSE);
    PSNR = 10*log10(256*256/MSE);
 
    set(handles.edit1,'String',MSE)
    set(handles.edit2,'String',RMSE)
    set(handles.edit3,'String',PSNR)
 
    val2 = get(handles.popupmenu2,'Value');
 
    switch val2
        case 1
            Img_filter = imfilter(Img_noise,ones(3)/9);
        case 2
            Img_filter = imfilter(Img_noise,ones(5)/25);
        case 3
            Img_filter = medfilt2(Img_noise,[3 3]);
        case 4
            Img_filter = medfilt2(Img_noise,[5 5]);
    end
 
    axes(handles.axes3)
    imshow(Img_filter)
    title('Citra Hasil Restorasi')
 
    MSE = sum(sum((Img-Img_filter).^2))/(row*col);
    RMSE = sqrt(MSE);
    PSNR = 10*log10(256*256/MSE);
 
    set(handles.edit4,'String',MSE)
    set(handles.edit5,'String',RMSE)
    set(handles.edit6,'String',PSNR)
catch
end

