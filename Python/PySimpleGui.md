Youtube Channel
https://www.youtube.com/@TheCSClassroom


```python
import PySimpleGUI as sg

# configure the theme
sg.ChangeLookAndFeel('DarkGrey15') 

```


# 组件

### Output Text Area

Only plain text, cannot print colorful text
```python
# used for log
sg.Output(size=(50, 32), font=("consolas", 11), key='-Output-')
window['-Output-'].update(content) # print to output
```


### 超链接

```python
```python
import webbrowser
import PySimpleGUI as sg

urls = {
    'Google':'https://www.google.com',
    'Amazon':'https://www.amazon.com/',
    'NASA'  :'https://www.nasa.gov/',
    'Python':'https://www.python.org/',
}

items = sorted(urls.keys())

sg.theme("DarkBlue")
font = ('Courier New', 16, 'underline')

layout = [[sg.Text(txt, tooltip=urls[txt], enable_events=True, font=font,
    key=f'URL {urls[txt]}')] for txt in items]
window = sg.Window('Hyperlink', layout, size=(250, 150), finalize=True)

while True:
    event, values = window.read()  # value中可以区到用户的输入
    if event == sg.WINDOW_CLOSED:
        break
    elif event.startswith("URL "):
        url = event.split(' ')[1]
        webbrowser.open(url)
    print(event, values)

window.close()
```
