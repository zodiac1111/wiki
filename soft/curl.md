# http下载文件小例子

c/c++实现.

```c
#include <stdio.h>
#include <curl/curl.h>
static size_t write_data(void *ptr, size_t size, size_t nmemb, void *stream)
{
	int written = fwrite(ptr, size, nmemb, (FILE *) stream);
	return written;
}

char *get_fname_from_url(char *url){
	char *p = url;
	while (*p)
		p++;
	p--;
	while (1) {
		if (*p=='/')
			break;
		else
			p--;
		
	}
	return ++p;
}

int main(int argc, char **argv)
{
	CURL *curl;
	CURLcode res;
	char *url;
	FILE *fp;
	if (argc==2)
		url = argv[1];
	else {
		return 0;
	}
	curl = curl_easy_init();
	if (curl) {
		fp = fopen(get_fname_from_url(url), "w");
		if (fp==NULL) {
			curl_easy_cleanup(curl);
			return 1;
		}
		curl_easy_setopt(curl, CURLOPT_URL, url);
		curl_easy_setopt(curl, CURLOPT_NOPROGRESS, 1L);
		curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, write_data);
		curl_easy_setopt(curl, CURLOPT_WRITEDATA, fp);
		/* Perform the request, res will get the return code */
		res = curl_easy_perform(curl);
		/* Check for errors */
		if (res!=CURLE_OK)
			fprintf(stderr, "curl_easy_perform() failed: %s\n",
			curl_easy_strerror(res));
		fclose(fp);
		/* always cleanup */
		curl_easy_cleanup(curl);
	}
	return 0;
}
```