# Shuffle SDK
This is the SDK used for app development, testing and production of ALL apps in Shuffle. Works with manual runs, Docker, k8s, cloud serverless. 

Released under [Python pip for usage outside of Shuffle](https://pypi.org/project/shuffle-sdk/) 

Python apps: [https://github.com/shuffle/python-apps](https://github.com/shuffle/python-apps)
All apps: [https://shuffler.io/search](https://shuffler.io/search)

## Usage with Shuffle
Refer to the [Shuffle App Creation docs](https://shuffler.io/docs/app_creation)

## Build
`docker build . -t shuffle/shuffle:app_sdk`

## Download
```
pip install shuffle_sdk
```

## Usage
```python
from shuffle_sdk import AppBase

class Example(AppBase):
    def __init__(self):
        pass

    def sample_function(self, paramname):
        return f"Hello {paramname}"

if __name__ == "__main__":
    Example.run()
```

## Testing Shuffle Apps
With the above function as an example
```bash
python3 app.py --standalone --action=sample_function paramname=World
```

Example with Liquid and the [Shuffle Tools app and the "repeat back to me" function](https://github.com/Shuffle/python-apps/blob/678187d1198f5e8fd2072e475dbbbf858728dde8/shuffle-tools/1.2.0/src/app.py#L235)
```bash
python3 app.py --standalone --action=repeat_back_to_me '--call={{ "hello" | replace: "o", "lol" }}'
```

Example using [Shuffle actions](https://github.com/shuffle/shufflepy) within the "execute_python" function to get emails from Outlook ([app.py](https://github.com/Shuffle/python-apps/blob/678187d1198f5e8fd2072e475dbbbf858728dde8/shuffle-tools/1.2.0/src/app.py#L570))
```bash
python3 app.py --standalone --action=execute_python 'code=print(shuffle.run_app(app_id="accdaaf2eeba6a6ed43b2efc0112032d", action="get_emails"))'
```

If successful, the output of the function will show in your CLI.

## Building a fully functional Shuffle App
[Look at the documentation on our website](https://shuffler.io/docs/app_creation)

## Adding new [Liquid filters](https://shuffler.io/docs/liquid)
Add a function along these lines:
```
@shuffle_filters.register
def md5(a):
    a = str(a)
    return hashlib.md5(a.encode('utf-8')).hexdigest()
```

This can be used as `{{ "string" | md5 }}`, where `"string"` -> the `a` parameter of the function
