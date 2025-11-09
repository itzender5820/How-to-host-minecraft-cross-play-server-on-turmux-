# How-to-host-minecraft-cross-play-server-on-turmux-
Here I'm gonna show you how you can host a minecraft crossplay server using just turmux 
1. install basics (termux)



pkg update -y && pkg upgrade -y

pkg install openjdk-21 wget nano -y

2. make server folder



mkdir -p ~/minecraft/plugins
cd ~/minecraft

3. download PaperMC 1.21.8 (build 60)



wget https://api.papermc.io/v2/projects/paper/versions/1.21.8/builds/60/downloads/paper-1.21.8-60.jar -O paper.jar

4. first run to generate files (will create eula.txt etc)



java -Xmx1G -Xms1G -jar paper.jar nogui

then stop with Ctrl+C

5. accept EULA



nano eula.txt

change eula=false to eula=true then save (CTRL+O, Enter, CTRL+X)

6. download plugins (put in plugins folder)



cd ~/minecraft/plugins
wget https://download.geysermc.org/v2/projects/geyser/versions/latest/builds/latest/downloads/spigot -O Geyser-Spigot.jar
wget https://download.geysermc.org/v2/projects/floodgate/versions/latest/builds/latest/downloads/spigot -O Floodgate-Spigot.jar
wget https://github.com/ViaVersion/ViaVersion/releases/download/5.5.1/ViaVersion-5.5.1.jar -O ViaVersion.jar

7. start server (normal foreground)



cd ~/minecraft
java -Xmx6G -Xms1G -jar paper.jar nogui

watch console until Done (xxs)! For help, type "help"

8. check Geyser generated config



ls plugins/Geyser-Spigot
nano plugins/Geyser-Spigot/config.yml

set minimal values (example entries)

bedrock:
  address: 0.0.0.0
  port: 19132
remote:
  address: x.x.x.x ( note : you have to put your local ip here to check "ifconfig" in terminal )
  port: 25565
  auth-type: floodgate

save and exit.

9. restart server to load updated config



pkill -f paper.jar
nohup java -Xmx6G -Xms1G -jar paper.jar nogui > nohup.out 2>&1 &
tail -f nohup.out

look for

[Geyser-Spigot] Started Geyser on UDP port 19132

10. Floodgate notes



Floodgate will create a keys folder and a key file (key.pem). Keep it safe.

Bedrock players can join using your phone IP and port 19132 once Floodgate/auth is ready.


11. helpful commands



ps aux | grep java          # check running java
pkill -f paper.jar          # stop server
tail -n 200 nohup.out       # see recent log
rm -rf ~/minecraft          # wipe and start over (BE CAREFUL)

Useful links (paste in browser or wget)

PaperMC 1.21.8 build 60: https://api.papermc.io/v2/projects/paper/versions/1.21.8/builds/60/downloads/paper-1.21.8-60.jar

Geyser-Spigot download: https://download.geysermc.org/v2/projects/geyser/versions/latest/builds/latest/downloads/spigot

Floodgate-Spigot download: https://download.geysermc.org/v2/projects/floodgate/versions/latest/builds/latest/downloads/spigot

ViaVersion 5.5.1: https://github.com/ViaVersion/ViaVersion/releases/download/5.5.1/ViaVersion-5.5.1.jar
