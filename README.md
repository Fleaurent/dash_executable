# dash_executable
build executable dash app with pyinstaller

## main project structure

- app.py
```python
import dash
import dash_core_components as dcc
import dash_html_components as html


# 1. reusable components
def resource_path(relative_path: str) -> os.path:
    """ Get absolute path to resource, works for dev and for PyInstaller """
    base_path = getattr(sys, '_MEIPASS', os.path.dirname(os.path.abspath(__file__)))

    return os.path.join(base_path, relative_path)


# 2. instantiate web app
if getattr(sys, 'frozen', False):
  # add_folder = resource_path('folder')
  app = dash.Dash(__name__)
else:
  app = dash.Dash(__name__)


# 3. app.layout
app.layout = html.Div([html.H2("Text"),
                       dcc.Input(id='my-id', value='initial value', type='text'),
                       html.Div(id='my-div')])


# 4. callbacks 
@app.callback(Output(component_id='my-div', component_property='children'),
              [Input(component_id='my-id', component_property='value')])
def update_output_div(input_value):
    return 'You\'ve entered "{}"'.format(input_value)


# 5. start webapp
def open_browser():
    if os.environ.get('WERKZEUG_RUN_MAIN') != 'true':
        # do something only once, before the reloader
        webbrowser.open_new('http://127.0.0.1:8050/')
        

def main():
  Timer(1, open_browser).start()
  app.run_server(debug=False)


if __name__ == '__main__':
	main()
```

## add hooks manually
[source](https://www.soinside.com/question/qdgtPrbssusUgSieXqXsij)  
[source repository](https://github.com/verdi07/jiostocks)  
- hooks/hook-dash_bootstrap_components.py
- hooks/hook-core_components.py
- hooks/hook-dash_html_components.py
- hooks/hook-dash_renderer.py

## build executable
$ pyinstaller --additional-hooks-dir=hooks app.py  

if onefile, add command line argument:  
--onefile  

if module error "pkg_resources.py2_warn" not found, add command line argument:  
--hidden-import=pkg_resources.py2_warn  

