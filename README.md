# libnginx-mod-http-fastdfs
FastDFS nginx module of dynamic.

## How to?
```
tar -zvxf libnginx-mod-http-fastdfs-6.12.3.tar.gz

[ -d /usr/include/fastcommon ] && \
[ -d /usr/include/fastdfs ] && \
[ -d /usr/include/sf ] && \
sed -i '/^tar .*.tar.xz/a \\t[ -d /usr/include/fastcommon ] && \\\n\t[ -d /usr/include/fastdfs ] && \\\n\t[ -d /usr/include/sf ] && \\\n\trm -fr src/http/modules/fastdfs/fastcommon \\\n\tsrc/http/modules/fastdfs/fastdfs \\\n\tsrc/http/modules/fastdfs/sf' ./nginx-add-ngx_http_fastdfs_module.patch.sh && \
sed -i '/^tar .*.tar.xz/a sudo cp src\/http\/modules\/fastdfs\/docs\/fastdfs.conf \/etc\/fdfs\/fastdfs.conf' ./nginx-add-ngx_http_fastdfs_module.patch.sh

sudo ./nginx-add-ngx_http_fastdfs_module.patch.sh
```

## Thanks
```
sudo apt install nginx-full
sudo apt install \
    fastdfs \
    fastdfs-server-dbgsym \
    fastdfs-tool-dbgsym \
    libfastcommon-dev \
    libfdfsclient-dbgsym \
    libfdfsclient-dev \
    libserverframe-dev

git clone https://github.com/happyfish100/libfastcommon.git
git clone https://github.com/happyfish100/libserverframe.git
git clone https://github.com/happyfish100/fastdfs.git
git clone https://github.com/happyfish100/fastdfs-nginx-module.git

cd fastdfs
./make.sh
tracker/fdfs_trackerd --version
sudo cp tracker/fdfs_trackerd /usr/bin/fdfs_trackerd
storage/fdfs_storaged --version
sudo cp storage/fdfs_storaged /usr/bin/fdfs_storaged
sudo systemctl restart fdfs_trackerd.service
sudo systemctl restart fdfs_storaged.service

sudo systemctl restart nginx.service
_var_lib_dpkg_status=$(fdfs_upload_file /etc/fdfs/client.conf /var/lib/dpkg/status)

curl http://localhost/${_var_lib_dpkg_status}|more
```
