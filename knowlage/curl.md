To see the headers from the response when using `curl`, you can use one of the following methods:

https://oxylabs.io/blog/curl-send-headers

```
### 1. Using the -I or --head Option
This option sends a HEAD request, which retrieves only the headers without the body of the response.

curl -I http://example.com

### 2. Using the `-i` or `--include` Option

This option includes the response headers in the output along with the body of the response.

`curl -i http://example.com`

### 3. Using the `-v` or `--verbose` Option
This option provides detailed information about the request and response, including both headers and body.


`curl -v http://example.com`

### 4. Saving Headers to a File

If you want to save the headers to a file for later inspection, you can use the `-D` option followed by a filename.

`curl -D headers.txt http://example.com`

This command will save the response headers to `headers.txt`.
```
