# Patrón de Diseño Adapter - Ejercicios de Programación Java

## 🎯 Objetivo de Aprendizaje

Comprender cómo el patrón Adapter permite que interfaces incompatibles trabajen juntas creando un puente entre ellas.
---
## 🔧 Step-by-Step Transformation Instructions

### 1. Identify the Incompatibility Problem

The AudioPlayer can only play MP3 files
We have AdvancedMediaPlayer implementations for VLC and MP4
The interfaces are incompatible

### 2. Create the MediaAdapter Class

Implement the MediaPlayer interface
Add a field for AdvancedMediaPlayer
Create a constructor that accepts audioType and initializes the appropriate player

```java
public class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedMusicPlayer;
    
    public MediaAdapter(String audioType) {
        if(audioType.equalsIgnoreCase("vlc")) {
            advancedMusicPlayer = new VlcPlayer();
        } else if(audioType.equalsIgnoreCase("mp4")) {
            advancedMusicPlayer = new Mp4Player();
        }
    }
    
    
}
```
### 3. Implement the play Method in MediaAdapter

Delegate the call to the appropriate method in AdvancedMediaPlayer
Handle different audio types (vlc, mp4)

```java
    @Override
    public void play(String audioType, String fileName) {
        if(audioType.equalsIgnoreCase("vlc")) {
            advancedMusicPlayer.playVlc(fileName);
        } else if(audioType.equalsIgnoreCase("mp4")) {
            advancedMusicPlayer.playMp4(fileName);
        }
    }
```

### 4. Modify AudioPlayer to Use the Adapter

Add a condition to use MediaAdapter for supported advanced formats
Maintain existing MP3 functionality

```java
public class AudioPlayer implements MediaPlayer {
    private MediaAdapter mediaAdapter;
    
    @Override
    public void play(String audioType, String fileName) {
        if(audioType.equalsIgnoreCase("mp3")) {
            System.out.println("Playing mp3 file: " + fileName);
        } else if(audioType.equalsIgnoreCase("vlc") || audioType.equalsIgnoreCase("mp4")) {
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType, fileName);
        } else {
            System.out.println("Invalid media type: " + audioType);
        }
    }
}
```
### 5. Test the Complete Solution

Create a test class to verify all media types work correctly

```java
public class AdapterPatternDemo {
    public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();
        
        audioPlayer.play("mp3", "song.mp3");
        audioPlayer.play("mp4", "video.mp4");
        audioPlayer.play("vlc", "movie.vlc");
        audioPlayer.play("avi", "movie.avi");
    }
}
```