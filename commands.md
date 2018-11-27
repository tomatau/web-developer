# Commands

- `du` : Directory Size
- `npm prune --production` : Remove dev dependencies
- `sips -Z 640 *.jpg`: make all jpg files in folder 640 max height
- `travis encrypt $(heroku auth:token) --add deploy.api_key`

**.Mov to .Gif**

`ffmpeg -i in.mov -s 600x400 -pix_fmt rgb24 -r 10 -f gif - | gifsicle --optimize=3 --delay=3 > out.gif`

https://gist.github.com/dergachev/4627207
