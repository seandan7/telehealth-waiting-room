<template>
  <div class="th-container">
    <div id="video-container"></div>
    <div id="videos-small"></div>
  </div>
</template>
<script>
import Video from "twilio-video";
export default {
  name: "Telehealth",

  data() {
    return {
      isActive: false,
      room: null
    };
  },
  props: {
    name: {
      required: true,
      default: null
    }
  },
  async mounted() {
    let token = null;
    await fetch(
      `http://localhost:8081/token?roomName=room12&identity=${this.name}`
    )
      .then(data => {
        return data.text();
      })
      .then(body => {
        token = body;
      });

    Video.connect(token, {
      name: "room12",
      audio: true
    })
      .then(function(room) {
        var button = document.querySelectorAll(
          ".door-container .modal-default-button"
        )[0];
        button.addEventListener("click", () => {
          console.log("Disconnected");
          room.disconnect();
        });

        Video.createLocalVideoTrack({ video: { width: 200 } }).then(track => {
          const localMediaContainer = document.getElementById("videos-small");
          localMediaContainer.appendChild(track.attach());
          return room.localParticipant.publishTrack(track);
        });
        return room;
      })
      .then(room => {
        window.onbeforeunload = function(evt) {
          room.disconnect();

          // Google Chrome requires returnValue to be set
          evt.returnValue = "";

          return null;
        };
        room.participants.forEach(participant => {
          console.log(participant.identity + "is already here");
        });
        room.on("participantConnected", participant => {
          console.log(participant.identity + " has connected");

          this.addRemoteTracks(participant);
        });

        room.once("disconnected", () => {
          console.log("You left the Room:", room.name);
        });
        room.on("participantDisconnected", participant => {
         this.removeRemoteTracks(participant.sid)
        });
      });
  },
  methods: {
    addRemoteTracks(participant) {
      participant.tracks.forEach(publication => {
        if (publication.isSubscribed) {
          const track = publication.track;
          const newElement = document.createElement("div");
          newElement.id = participant.sid
          newElement.appendChild(track.attach())
          console.log(newElement)
          document
            .getElementById("video-container")
            .appendChild(newElement);
        }
      });

      participant.on("trackSubscribed", track => {
         const newElement = document.createElement("div");
          newElement.id = participant.sid
          newElement.appendChild(track.attach())
          console.log(newElement)
          document
            .getElementById("video-container")
            .appendChild(newElement);
      });
    },
    removeRemoteTracks(sid) {
      
      const elementToRemove = document.getElementById(sid)
      elementToRemove.parentNode.removeChild(elementToRemove)
    }
  }
};
</script>
<style>
#video-container {
  position: relative;
  display: flex;
}
#videos-small {
  position: absolute;
  width: 100%;
  height: 100px;
  bottom: 15px;
  left: 0;
  display: flex;
  justify-content: center;
}
#videos-small video {
  width: 99px;
  height: 75px;
  border: 2px solid black;
}
#video-container video {
  max-width: 100%;
}
</style>
