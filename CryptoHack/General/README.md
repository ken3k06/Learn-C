# ASCII
<img width="1537" height="530" alt="image" src="https://github.com/user-attachments/assets/814d1716-0c67-492c-a186-e42e957fd069" />

Bài này yêu cầu ta chuyển mã ASCII sang kí tự và nối lại để thành flag. 
Solve: 
```c
#include <stdio.h>
#include <stdlib.h>
int main(){

    int c[] = {99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125};
    int len = sizeof(c) / sizeof(c[0]); 
    printf("%d\n", len); 
    char *flag = (char *)malloc(len +1); 
    for (int i = 0; i < len ; i++){
        *(flag + i) = (char)c[i];
    }
    flag[len] = '\0'; 
    printf("%s\n", flag); 
    return 0;
}
```
# Hex 
Bài này cho ta flag dưới dạng hex, nhiệm vụ của ta là chuyển về dạng bytes. 
Cơ chế để chuyển từ hex thành bytes là gì? 

1 byte sẽ có độ lớn là 8 bit và tương ứng với 2 kí tự hex. Cho nên trước khi chuyển hex về bytes ta cần kiểm tra xem độ dài của nó có chia hết cho 2 hay không. 

Tiếp đó ta sẽ lấy từng bộ 2 kí tự hex, tính toán ra giá trị thập phân của nó rồi chuyển về char. 
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#include <stdint.h> 
int hex_value(char c){
    if ('0'<=c && c<='9') return c - '0'; 
    if ('a'<=c && c <='f') return c - 'a' + 10;
    if ('A'<=c && c <= 'F') return c - 'A' + 10; 
}

// unsigned char * hex_to_bytes(const char *hex, size_t *out_len){
//     size_t len = strlen(hex); 
//     if (len %2 != 0) return NULL; 
//     *out_len = len/2; 
//     unsigned char *buf = malloc(*out_len);
// }

uint8_t* hex_to_bytes(const char *hex, size_t *out_len){
    size_t len = strlen(hex); 
    *out_len = len/2; 
    uint8_t *buf = (uint8_t*)malloc(*out_len); 
    for (int i = 0 ; i <*out_len; i++){
        buf[i] = (hex_value(hex[2*i]) << 4) | hex_value(hex[2*i+1]); 
    }
    return buf;
}


int main(){
    char hex[] = "63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d";
    size_t len; 
    uint8_t *b = hex_to_bytes(hex, &len);
    printf("%s\n", (char*)b); 
    return 0;
}
```

Note: Cách chuyển về cơ bản sẽ là 
```c
buf[i] = (hex_value(hex[2*i]) << 4) | hex_value(hex[2*i+1])
```

Còn để chuyển từ bytes về hex: 
```c
void bytes_to_hex(const uint8_t *bytes, size_t len, char* out){
    static const char hex[] = "0123456789ABCDEF"; 
    for (int i = 0 ; i < len; i++){
        out[2*i] = hex[(bytes[i] >> 4) & 0xF]; 
        out[2*i+1] = hex[(bytes[i]) & 0xF];
    }
    out[2*len] = '\0'; 
}
```
