---
title: 'Data Uploads'
excerpt: 'Scripting examples on how to execute a load test that will upload a file to the System Under Test(SUT).'
---

Example to execute a load test that will upload a file to the System Under Test(SUT).

## The open() function

Using the built-in function, [`open()`](/javascript-api/init-context/open-filepath-mode),
we are able to read the contents of a file given a filename or URL.

Below is a simple example showing how to load the contents of a local file `data.json`.

<CodeGroup labels={["data.json"]} lineNumbers={[true]}>

```json
{
  "my_key": "has a value"
}
```

</CodeGroup>

<CodeGroup labels={["Loading a local JSON file using open()"]} lineNumbers={[true]}>

```javascript
const data = JSON.parse(open('./data.json'));

export default function () {
  console.log(data.my_key);
}
```

</CodeGroup>

If you want to open a binary file you need to pass in `"b"` as the second argument.

<CodeGroup labels={["Loading a binary file using open()"]} lineNumbers={[true]}>

```javascript
const binFile = open("./image.png", "b");

export default function() {
    ...
}
```

</CodeGroup>

## Multipart request (uploading a file)

Now that you know how to load a local file, let's look at a script that creates a POST request
to upload this data to an API endpoint along with a regular text field (`field` in the example
below):

<CodeGroup labels={["POST upload example"]} lineNumbers={[true]}>

```javascript
import http from 'k6/http';
import { sleep } from 'k6';

let binFile = open('/path/to/file.bin', 'b');

export default function () {
  var data = {
    field: 'this is a standard form field',
    file: http.file(binFile, 'test.bin'),
  };

  var res = http.post('https://example.com/upload', data);
  sleep(3);
}
```

</CodeGroup>

In the example above we use the [http.file()](/javascript-api/k6-http/file-data-filename-contenttype)
API to wrap the file contents in a [FileData](/javascript-api/k6-http/filedata) object.
When passing a JS object as the body parameter to [http.post()](/javascript-api/k6-http/post-url-body-params),
or any of the other HTTP request functions, where one of the property values is a
[FileData](/javascript-api/k6-http/filedata) a multipart request will be constructed
and sent.

### Relevant k6 APIs

- [open(filePath, [mode])](/javascript-api/init-context/open-filepath-mode)
- [http.file(data, [filename], [contentType])](/javascript-api/k6-http/file-data-filename-contenttype)
