# Flask
Flask is a lightweight framework for backend development in Python.  This is a short introduction to the mandatory flask components you will use in the development.  I **strongly recommend** people read another [wiki](https://github.com/UT-Applicataion-Development-Association/st-backend/wiki/REST-API) before reading the remainder of this post.

Here is the list of contents in this post:
- [Server](#Server)
- [Route](#Route)
- [Blueprint](#Blueprint)
- [CORS](#CORS)
- [Config & Environment Variable](#Config-And-Environment-Variable)

## Server
A **server** is a process that **keeps running for listening to the client's request and sending the response for the request**.  (This is essentially a loop with the condition of While True, you will explore the principle behind a server in CSC209 and CSC458, we will not go into any more detail on the principle).

In Flask, a server is nothing, but a Flask instance.  Every app should only have one server running meaning that **you should not create any new instantiation of the server other than one in the entry point**.
```py
# How to create a Flask server
from flask import Flask
# This creates a Flask server
app = Flask(__name__):
if __name__ == '__main__':
    # You can turn the optional parameter for debug and running port of the server
    app.run()
```

## Route
The route is the actual component in the Flask server where listening to the request(**resource action** mentioned in another wiki) send to the URI (the **resource** mentioned in another wiki)and handles the request(this is sometimes called the handler).  Evert route must have an associate response function for handling the request.

- Create a route
```py
from flask import Flask, request
app = Flask(__name__)
# The flask route is done through decorator @route
# An example of listening to the index URI
@app.route('/')
def index_response():
    # Write code to handle the request
```

- Add method to a route
```py
# By default the route only support the GET method
# To include more methods on a route, you need to include methods argument in the decorator
from flask import Flask, request
app = Flask(__name__)
@app.route('/demo', methods=['GET', 'POST'])
def demo_response():
    # To examine which type of the request, you need to use the request imported above
    if request.method == 'GET':
        # Write code for handling get request
    elif request.method == 'POST':
        # Write code for handling post request
```

- Add path parameter
```py
# To support a path parameter, you need to specify the path parameter in the route
# And put it into the response handler the name should exactly same
from flask import Flask, request
app = Flask(__name__)
@app.route('/user/<userId>')
def user_response(userId):
    # Handle it
```

- Access query parameter
```py
# You do not need to specify query parameter, you can obtain query parameters by the request's args
# Suppose we have a route of /imgs?date=<test date>
# Remember the date is the query parameter, not the imgs
from flask import Flask, request
app = Flask(__name__)
@app.route('/imgs')
def img_response():
    d = request.args.get('date')
    # Handle it
```

- Access data in the request
```py
# Last but not least, you can use the request to obtain the data sending to the backend
# There are two ways for accessing the data
# The usages differ on the representation of the resource
# Typically we will use either the JSON or form to represent the resource
# Both of them are stored as the dictionary in the request
from flask import Flask, request
app = Flask(__name__)
@app.route('/data', methods=['POST'])
def data_response():
    # To access data in the JSON
    json_data = request.json
    # To access data in the Form
    form_data = request.form
    # Handle it
```

## Blueprint
Informally, the blueprint is an advanced usage of the route.  In short words, we can separate multiple routes into different blueprints for a better design.

Suppose I have two major routes `/user/...` and `/admin/...`.  Assume there are many potential sub-routes of them.

One way of writing routes is to make all the routes in the same file under the server.
```py
from flask import Flask
app = Flask(__main__)
@app.route('/user')
def user_response():
    # Handle it
#Sub routes for the user route

@app.route('/admin')
def admin_response():
    # Handle it
#Sub routes for the admin route
```

Frankly, this is a bad design.  It makes the server super messy and hard to manage different routes.  The rule of thumb in web design is to make every file be **as simple as possible**.  Introducing an almighty class is a widely-seen bad design for beginners.  Blueprint is introduced to solve this problem.

```py
# User Blueprint
from flask import request, BluePrint
# create a blueprint
userBluePrint = BluePrint('<write name of the route>', __name__)

# Listening to the route
# Every blueprint has no knowledge of root structure or hierarchy
# Every blueprint assumes that they are the root of the resource
@userBluePrint.route('/')
def response():
    # Handle it
#Sub routes for the user
```

Similarly, we can create another blueprint for admin
```py
# Admin Blueprint
from flask import request, BluePrint
# create a blueprint
adminBluePrint = BluePrint('<write name of the route>', __name__)

# Listening to the route
# Every blueprint has no knowledge of root structure or hierarchy
# Every blueprint assumes that they are the root of the resource
@adminBluePrint.route('/')
def response():
    # Handle it
#Sub routes for the admin
```

Although blueprints have no knowledge of the structure, the server does.  The server's job is to register those blueprints and assign each of them a prefix.
```py
# Sever
from flask import Flask
import <file stores userBluePrint> as user
import <file stores adminBluePrint> as admin

# Note we mentioned that every route treat them as the root of the route
# The reason for that is because we are assigning them the prefix
# Thus, blueprints can and should assume they are the root of the resource
app.register_blueprint(user.userBluePrint, prefix='/user')
app.register_blueprint(admin.adminBluePrint, prefix='/admin')
```

## CORS
CORS is known as the **cross-origin resource sharing**, this is a commonly-seen phenomenon in web design.  Historically, the request is only allowed to be sent if the request happens in the **"same origin"** (mainly for safety reasons).
There are three components to define an origin:
- Protocol

- Port

- Domain

If any one of three does not match, then the request is treated as **cross-origin**, and the browser blocks it.  Nevertheless, the need for cross origin never goes down, especially for the RESTful design.  Thus, CORS is introduced.  The reason why do we need to use CORS is that our frontend and backend are running on different ports which will cause the browser to block the request if we do not enable the CORS.  This is all you need to know for the CORS at this moment, if you want to know more about this you can check some online materials, we will not go into any detail about the mechanism of the CORS at this moment.

```py
# To enable the cors we need the flask-cors
# flask-cors is not a part of the flask
# it requires additional installation
from flask-cors import CORS, cross_origin
from flask import Flask
app = Flask(__name__)
cors = CORS(app)

# Adding the cross_origin after the route to enable the cors on the route/blueprint
@app.route('/demo')
@cross_origin()
def response():
    # Handle it
```

## Config And Environment Variable
The Flask object has an attribute named config which is a dictionary store the configuration for the server.  The configuration includes but not limited to mode, session_time, secret, database_uri, etc.  Mostly, we do not need to change the config during the running, but in some cases, we do actually need to update the config.

Updating a config is an easy job you can do this by using `app.config.update(new_config)`.   The `new_config` is a dictionary for updating, the common configs that exist in both `old_config` and `new_config` will be updated accordingly to the `new_config`.  Also, it will insert into `old_config` from `new_config` if they are not presented in the `old_config`

Also, there are some cases we need to introduce some additional configs to the server when we set up it in the beginning.  You can definitely explicitly write them in your setup.  Here, we will introduce a new and more professional way of doing it that is using the environment variable.

Briefly, the environment variables are additional or special variables for a process.  Those are followed along of a process and recall that the server is also a running process, thus we can let sever read and use those variables.

The detailed workflow is:

1. We will store environment variables in a file named `.env` (Normally, the env file is encrypted but for simplicity, we will not do it now, this will be added in the future)

2. To load those environment variables a library named `dotenv` is needed

3. To read the environment variables the `environ` from Python's default os library is needed

4. We will store environment variables in the class

5. Update server config using `from_object`
