# YT Downloads and Plex setup

Follow the instructions from here to install both the scanner and the agent:

- https://github.com/JordyAlkema/Youtube-DL-Agent.bundle
- https://github.com/ZeroQI/Absolute-Series-Scanner

Use YTDL to download videos likes this:

```bash
ydl https://youtu.be/56_8aK3cLEA --add-metadata --write-info-json --write-thumbnail -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/mp4' -o "./youtube/%(uploader)s [%(uploader_id)s]/%(title)s[%(id)s].%(ext)s"
```
