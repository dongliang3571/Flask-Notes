# Flask-Notes

## Request Decorator

**Request decorator** 

- @before_request

This decorator to registers a function to run before each request. It will apply to all requests within the app, if you have no other blueprints. If you have one or more blueprints, this decorator will only apply to particular blueprint

Implementation of `before_request` decorator:

```python
@setupmethod
def before_request(self, f):
"""Registers a function to run before each request."""
    self.before_request_funcs.setdefault(None, []).append(f)
    return f
```

Note: `self.before_request_funcs` is an attribute of dictionary type in `Flask` class, key in this dict will be blueprint name. So all functions in `self.before_request_funcs` will be called.

Example of using it

```python
app = Flask(__name__)

@app.before_request
def before_request():
    g.user = None
    if 'user_id' in session:
        g.user = User.query.get(session['user_id'])
        
@app.route('/'):
    return 'test 1'
        
@app.route('/test2'):
    return 'test 2'
  
# all requests in all routes will call before_request before goes to view functions
```

after declaration of `def before_request()`, all other views function will call `before_request` before they get to run.

- @after_request

This decorator to registers a function to run after each request

- @before_app_request

This decorator will apply to all requests within the app, no matter how many blueprints you have. But this method only belongs to `Blueprint` class. Not in `Flask`.

