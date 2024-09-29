# sdpaper
A repo to help people set up their own stable diffusion backgrounds

## Usage
**Press F1 to load new background**\
\
**Press F12 to save to a separate folder forever**\
\
For all configs below, replace $USER with your username, and replace the monitor values according to this guide: [monitors](https://wiki.hyprland.org/Configuring/Monitors/)


## Prerequisites
This project uses - **docker, hyprland, hyprpaper**\
\
Along with this repo - **[stable-diffusion-docker](https://github.com/fboulnois/stable-diffusion-docker)**\
\
Follow their respective documentation for install guides and troubleshooting. \
\
If you can run this example from the github link above, you should be set:\
`./build.sh run 'Andromeda galaxy in a bottle'`


## Hyprland Config
```
# AI Background Controls
bind = $mainMod SHIFT, F1, exec, $terminal -e /home/$USER/.config/hypr/sd_paper.sh 
bind = $mainMod SHIFT, F12, execr, cp /home/$USER/.config/wallpapers/anime.png /home/$USER/.config/wallpapers/saves/$(sha256sum /home/$USER/.config/wallpapers/anime.png | cut -d " " -f 1).png
```


## Hyprpaper Config
This sets the background image for when hyprpaper is first launched. You most likely will have to change the monitors to fit your needs
```
preload=/home/$USER/.config/wallpapers/anime.png
wallpaper = DP-2,/home/$USER/.config/wallpapers/anime.png
wallpaper = HDMI-A-1,/home/$USER/.config/wallpapers/anime.png

splash=false
```


## sd_paper.sh
```
#!/bin/bash

export USER = "bob"
export SDD_PATH = /PATH/TO/stable-diffusion-docker

cd $SDD_PATH

# Beach Girl
./build.sh run --width 1280 --height 720 --output anime.png --vae-tiling --model 'dreamlike-art/dreamlike-anime-1.0' --prompt 'anime, masterpiece, high quality, alone, 1girl, solo, long hair, looking at viewer, blush, smile, bangs, blue eyes, swimsuit, medium breasts, iridescent, gradient, colorful, at the beach, blurred background' --negative-prompt 'hands, hand, palm, fingers, simple background, duplicate, retro style, low quality, lowest quality, 1980s, 1990s, 2000s, 2005 2006 2007 2008 2009 2010 2011 2012 2013, bad anatomy, bad proportions, extra digits, lowres, username, artist name, error, duplicate, watermark, signature, text, extra digit, fewer digits, worst quality, jpeg artifacts, blurry, group' --steps 60

# Future Skyline
#./build.sh run --width 1280 --height 720 --output future.png --vae-tiling --model 'dreamlike-art/dreamlike-anime-1.0' --prompt 'anime, future style city skyline at night, plants, flora' --negative-prompt 'people, blurry, fog, soft, hands, hand, palm, fingers, simple background, duplicate, retro style, low quality, lowest quality, 1980s, 1990s, 2000s, 2005 2006 2007 2008 2009 2010 2011 2012 2013, bad anatomy, bad proportions, extra digits, lowres, username, artist name, error, duplicate, watermark, signature, text, extra digit, fewer digits, worst quality, jpeg artifacts, blurry, group' --steps 60

cp $SDD_PATH/output/anime.png /home/$USER/.config/wallpapers/anime.png

echo "Reloading hyprpaper"
hyprctl hyprpaper unload "/home/$USER/.config/wallpapers/landscape.png"
hyprctl hyprpaper preload "/home/$USER/.config/wallpapers/anime.png"
echo "Refreshing Monitors"
hyprctl hyprpaper wallpaper DP-2,/home/$USER/.config/wallpapers/anime.png
hyprctl hyprpaper wallpaper HDMI-A-1,/home/$USER/.config/wallpapers/anime.png
```

## Credits
Also give some love to [dreamlike-art](https://huggingface.co/dreamlike-art/dreamlike-anime-1.0) for the model
