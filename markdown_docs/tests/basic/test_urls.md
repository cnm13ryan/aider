## FunctionDef test_urls
**test_urls**: The function of test_urls is to verify that all public attributes (excluding callable ones) from the `urls` module return HTTP 200 status codes when accessed via GET requests.
**parameters**: This Function does not take any parameters.
**Code Description**: 
The function `test_urls` performs a series of HTTP GET requests on various attributes from the `urls` module and checks their response status codes. Here is a detailed breakdown:

1. **URL Attribute Collection**: The first step involves identifying all non-callable attributes (excluding those starting with double underscores, which are typically private or special methods) in the `urls` module using list comprehension.
   ```python
   url_attributes = [
       attr
       for attr in dir(urls)
       if not callable(getattr(urls, attr)) and not attr.startswith("__")
   ]
   ```

2. **Attribute Iteration and Request Execution**: For each attribute identified in the previous step, a GET request is made to the URL corresponding to that attribute.
   ```python
   for attr in url_attributes:
       url = getattr(urls, attr)
       response = requests.get(url)
   ```

3. **Status Code Validation**: After executing the GET request, the status code of the response is checked against 200 (HTTP OK).
   ```python
   assert response.status_code == 200, f"URL {url} returned status code {response.status_code}"
   ```
   If the status code does not match 200, an assertion error will be raised with a message indicating which URL failed and why.

**Note**: Ensure that the `requests` library is installed in your environment as it is used to perform HTTP GET requests. Additionally, the `urls` module should contain valid URLs for attributes to avoid assertion errors.
**Output Example**: The function does not return any value directly; instead, it raises an AssertionError if any URL returns a status code other than 200. For example:
```
AssertionError: URL http://example.com/api/v1/ returned status code 404
```
