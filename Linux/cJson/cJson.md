# cJSON 使用教程

cJSON开源地址：https://github.com/DaveGamble/cJSON

测试例子:

```json
{
	"reqType":	0,
	"perception":	{
		"inputText":	{
			"text":	"今天怎么样"
		},
		"selfInfo":	{
			"location":	{
				"city":	"深圳",
				"province":	"龙岗"
			}
		}
	},
	"userInfo":	{
		"apiKey":	"3cb5f7325b0b4afaab5b22e7835949b8",
		"userId":	"7163"
	}
}
```

```c
#include "stdio.h"
#include "stdlib.h"
#include "cJSON.h"
#include "string.h"

/**
 *  将内容写在文件里。
 */
int write_json( const char *filename , char * buffer, int size )
{
    FILE *fp;
    if( filename == NULL || buffer == NULL )
    {
        return -1;
    }

    fp = fopen(filename,"w+");
    if(fp == NULL)
    {
        printf("Open %s is failed \r\n", filename);
        return -1;
    }

    fwrite( (const char *)buffer, 1, size, fp);
    fclose(fp);
    return 0;
}

/**
 *  从文件时读取json文件，保留在buffer中。
 */
int read_json( const char * filename, char * buffer, int size )
{
    int fsize;
    FILE *fp;
    if( filename == NULL || buffer == NULL )
    {
        return -1;
    }
    fp = fopen(filename, "r");
    if(fp == NULL)
    {
        printf("Open %s is failed \r\n", filename);
        return -1;
    }

    fseek(fp, 0, SEEK_END);
    fsize = ftell(fp);
    fseek(fp, 0, SEEK_SET);
    if(size < fsize + 1)
    {
        printf("buffer is too smaller \r\n");
        fclose(fp);
        return -1;
    }
    fsize = fread(buffer, 1,fsize, fp );
    fclose(fp);
    return fsize;

}



int main(int argc, const char * argv[])
{
    printf("cJSON test \r\n");
    cJSON * root;
    cJSON *perception, *inputText, *text;
    char * out;
    char buffer[1024];
    int size;
    memset(buffer,0,sizeof(buffer));
    size = read_json("test.json", buffer, sizeof(buffer) );    
    //printf("%s\r\n", buffer);
    root = cJSON_Parse(buffer);
    if(root == NULL){
        printf("root is null\r\n");
        return -1;
    }
    perception =  cJSON_GetObjectItem(root,"perception");
    if(perception == NULL)
    {
        printf("perception is null\r\n");
    }
    inputText = cJSON_GetObjectItem( perception,"inputText");
    if(inputText == NULL)
    {
        printf("inputText is null\r\n");
    }
    text = cJSON_GetObjectItem(inputText,"text");
    if(text->type == cJSON_String)
    {
        printf("set text \r\n");
        cJSON_SetValuestring(text,"今天怎么样");
    }
  
    out = cJSON_Print(root);
    write_json("test.json", out, strlen(out));
    printf("%s\r\n", out);
    cJSON_Delete(root);
    free(out);

    return 0;
}
```

