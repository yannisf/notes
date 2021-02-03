# gphoto2 _or how to control a DSLR from Linux_

sudo apt-get install gphoto2 v4l2loopback-utils ffmpeg
sudo modprobe v4l2loopback exclusive_caps=1 max_buffers=2
gphoto2 --capture-image-and-download
gphoto2 --stdout --capture-movie | ffmpeg -i - -vcodec rawvideo -pix_fmt yuv420p -threads 0 -f v4l2 /dev/video0
