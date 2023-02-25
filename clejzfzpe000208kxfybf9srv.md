# What is Coupling?

Coupling refers to the degree to which one software component depends on another. It describes how closely connected two classes or modules are to each other. A component with high coupling is dependent on another component to work properly, while a component with low coupling is less dependent and more independent. Coupling as a topic isn't language specific!

* There are 2 types of coupling
    
    * Tight coupling: In this type of coupling, two classes or modules are highly dependent on each other. A change in one class or module can affect the other, and vice versa. This can lead to maintenance issues, as changes made to one module can cause unintended effects on other modules.
        
    * Loose coupling: In this type of coupling, two classes or modules are less dependent on each other. Changes made to one module have minimal effect on the other. This makes it easier to maintain and update the software.
        

With the help of an example, let us understand what are the disadvantages of tight coupling and why we need loose coupling. This doesn't mean loose coupling is an ideal solution and should be used all the time. This purely depends on the requirements.

```java
package com.bharathkalyans;


class VLCMedia {
    public void playVideo() {
        System.out.println("Playing in VLC!");
    }
}


class MP4Media {
    public void playVideo() {
        System.out.println("Playing in MP4!");
    }
}


class MKVMedia {
    public void playVideo() {
        System.out.println("Playing in MKV!");
    }
}


public class VideoPlayer {

    public static void main(String[] args) {
        VLCMedia object = new VLCMedia();
        VideoPlayer videoPlayer = new VideoPlayer();
        videoPlayer.playVideo(object);

    }

    public void playVideo(VLCMedia object) {
        object.playVideo();
    }
}
```

* If you closely observe the above example, if you want to change the media type from VLC to MP4 or MKV, we have to create a new instance again.
    
* Also, we have our Video player class is tightly coupled with the VLC Media Player class. In the future, if any new changes are introduced to our VLC class cant be easily reflected in our Video player class.
    
* Now let's see how to remove this tight coupling using **interfaces** and **dependency injection.**
    

```java
package com.bharathkalyans;


interface MediaType {
    public void playVideo();
}

class VLCMedia implements MediaType {
    public void playVideo() {
        System.out.println("Playing in VLC!");
    }
}


class MP4Media implements MediaType {
    public void playVideo() {
        System.out.println("Playing in MP4!");
    }
}


class MKVMedia implements MediaType {
    public void playVideo() {
        System.out.println("Playing in MKV!");
    }
}


public class VideoPlayer {

    MediaType mediaType;

    VideoPlayer(MediaType mediaType) {
        this.mediaType = mediaType;
    }

    public static void main(String[] args) {
        MediaType vlcMedia = new VLCMedia();
        // cna easily to change any other mediatype by changing the class on right side.
        //Eg. MediaType mp4Media = new MP4Media();
        VideoPlayer videoPlayer = new VideoPlayer(vlcMedia);
        videoPlayer.playVideo();
    }

    public void playVideo() {
        mediaType.playVideo();
    }

}
```

* Using loose coupling any changes to the codebase can be easily introduced.
    

---

Hope you liked this blog! You can connect with me at [bharathkalyans](https://twitter.com/bharathkalyans) .