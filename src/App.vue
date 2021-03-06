<template>
  <div>
    <!-- This is the intro disclaimer page -->
    <div class="intro" v-if="disclaimer">
      <h1>The Test</h1>
      <p>
        Welcome. The Test is a theatrical production that serves as a data
        collection tool for graduate research through the University of Glasgow.
        If you choose to proceed, your participation in the study would consist
        of watching a forty-minute piece of interactive theatre on your device,
        and voting on several plot points along the way. Your votes will be
        anonymously collected as research data. This data will be analysed
        comparing two different experiences of the same piece of theatre.
      </p>
      <br />
      <p>
        You will need to take a moment to read the
        <a
          target="_blank"
          href="https://docs.google.com/document/d/1dgmYaCgC6GZ_boEQhekzlAgLx3tdB-EE9ZLqzZb5nb0/edit"
          >Participant Information Sheet</a
        >
        and
        <a
          target="_blank"
          href="https://docs.google.com/document/d/1SfzOrUtSTSGDP73zWVMVdqoduUbPFc6kEOcck54u3No/edit"
          >Consent Form</a
        >
        and agree to their terms before you proceed. Because this is a research
        endeavor, you may not watch the show multiple times to select other
        endings. In order to watch this show, you agree that your choices are
        final.
      </p>
      <br />
      <p>Ready to watch?</p>
      <legend>
        Please confirm that you have done the following before clicking submit.
      </legend>
      <input id="plain-lang" type="checkbox" value="plain-lang" required />
      <label for="plain-lang"
        >I have read and agree to the information in the participant information
        sheet</label
      >
      <br />
      <input id="consent" type="checkbox" value="consent" required />
      <label for="consent"
        >I have read and agree to the terms outlined in the consent form</label
      >
      <br />
      <div @click="submitDisclaimer()">SUBMIT</div>
    </div>
    <!-- This is the after show page -->
    <div class="afterShow" v-if="afterShow">
      <h1>Thank you for watching The Test.</h1>
      <p>
        If you feel so inclined, please share it with people you think might be
        interested. As this is part of my dissertation, the more data, the
        better!
      </p>

      <div class="divider"></div>

      <h3>Artist's Note:</h3>
      <p>
        I debated adding this note at all, since one always hopes that their art
        speaks for them. But in this short little play, I tried to get into as
        many heads as I possibly could, and make decisions as open-ended as
        possible. I hope that, whatever choices you made in this play, you think
        about for a little while longer. Were you choosing the most interesting
        plot? If so, what makes that interesting? Were you choosing what you
        thought you would do? If so, why do you choose like that? I have
        opinions that I hope come through in the premise of this project if
        nothing else, but my biggest hope for this work is that it gives you
        something to think about and talk about that maybe you wouldn't have
        gotten to without it. We may not be peloton-ing for our lives today, but
        we certainly deserve a better world.
      </p>
      <p>-Elisabeth</p>
    </div>
    <!-- This is the actual show itself  -->
    <div v-if="!disclaimer && !afterShow">
      <iframe
        v-if="intro === true"
        id="start-Q1"
        width="560"
        height="315"
        :src="urls[step]"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
        @mouseover="timer(currentVideoLength)"
      ></iframe>

      <iframe
        v-if="intro === false"
        id="start-Q1"
        width="560"
        height="315"
        :src="urls[videoIndex][videoSubIndex]"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
        @mouseover="timer(currentVideoLength)"
      ></iframe>

      <!-- Initial question -->
      <div v-if="intro === true && isVideoComplete === true">
        <p><strong>Do you want the blue shirt or the white shirt?</strong></p>
        <p @click="selectShirt('Blue')" class="option">Blue</p>
        <p @click="selectShirt('White')" class="option">White</p>
      </div>

      <div v-if="step === 0 && intro === false && isVideoComplete === true">
        <p>
          <strong>Do you want to go to a bar or to go to a library??</strong>
        </p>
        <p @click="recordAnswer('Bar', step)" class="option">Bar</p>
        <p @click="recordAnswer('Library', step)" class="option">Library</p>
      </div>

      <div v-if="step === 1 && isVideoComplete === true">
        <p>
          <strong
            >Do you want to ask Jonah why he pushed back on Brett or tell Jonah
            not to argue with Brett?</strong
          >
        </p>
        <p @click="recordAnswer('Ask', step)" class="option">Ask why?</p>
        <p @click="recordAnswer('Tell', step)" class="option">
          Tell him to stop
        </p>
      </div>

      <div v-if="step === 2 && isVideoComplete === true">
        <p>
          <strong
            >Do you tell her what just happened with your brother or
            deflect?</strong
          >
        </p>
        <p @click="recordAnswer('Tell', step)" class="option">Tell her.</p>
        <p @click="recordAnswer('Deflect', step)" class="option">Deflect.</p>
      </div>

      <div v-if="step === 3 && isVideoComplete === true">
        <p>
          <strong
            >Do you think it’s better to commit a dangerous act for necessary
            change, or do you think violent change could be worse?</strong
          >
        </p>
        <p @click="recordAnswer('Do', step)" class="option">Do something.</p>
        <p @click="recordAnswer('Avoid', step)" class="option">
          Avoid violence.
        </p>
      </div>

      <div v-if="step === 4 && isVideoComplete === true">
        <p>
          <strong>
            <p>
              <strong>Do you help them or try to talk them out of it?</strong>
            </p>
          </strong>
        </p>
        <p @click="recordAnswer('Help', step)" class="option">Help them.</p>
        <p @click="recordAnswer('Talk', step)" class="option">
          Talk them down.
        </p>
      </div>

      <div v-if="step === 5 && isVideoComplete === true">
        <p><strong>Do you cover for the radicals or turn them in?</strong></p>
        <p @click="recordAnswer('Cover', step)" class="option">
          Cover for them.
        </p>
        <p @click="recordAnswer('Turn', step)" class="option">Turn them in.</p>
      </div>

      <div v-if="step === 6 && isVideoComplete === true">
        <p>
          <strong>
            <p><strong>Do you join her or let her go alone?</strong></p>
          </strong>
        </p>
        <p @click="recordAnswer('Join', step)" class="option">Join her.</p>
        <p @click="recordAnswer('Let', step)" class="option">
          Let her go alone.
        </p>
      </div>

      <div v-if="step === 7 && isVideoComplete === true">
        <p><strong>Do you blow up the centre or walk away?</strong></p>
        <p @click="recordAnswer('Blow', step)" class="option">
          Blow up the center.
        </p>
        <p @click="recordAnswer('Walk', step)" class="option">Walk away.</p>
      </div>
      <div v-if="step === 8 && isVideoComplete === true">
        <button @click="afterShow = true">Post-Show Bios</button>
      </div>
    </div>
  </div>
</template>

<script>
import { send } from "emailjs-com";

export default {
  name: "App",
  data() {
    return {
      afterShow: false,
      disclaimer: true,
      intro: true,
      mainPhase: false,
      currentVideoLength: 4800,
      isVideoComplete: false,
      step: 0,
      videoIndex: 0,
      videoSubIndex: 0,
      shirtSelection: "None",
      hasTimerStarted: false,
      urls: ["https://www.youtube-nocookie.com/embed/DDRONxsIudQ?controls=0"],
      whiteShirtUrls: [
        ["https://www.youtube-nocookie.com/embed/3TADGbs6pnc?controls=0"],
        [
          "https://www.youtube-nocookie.com/embed/fAO02kPjkaM?controls=0",
          "https://www.youtube-nocookie.com/embed/lOjodj73Ovc?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/xXjBFvgLTxM?controls=0",
          "https://www.youtube-nocookie.com/embed/jg5E4F9xlio?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/yC5oB_Z2-ZE?controls=0",
          "https://www.youtube-nocookie.com/embed/YGXZA-JQs3M?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/mvz-nh_-Uw8?controls=0",
          "https://www.youtube-nocookie.com/embed/Ks-9yLulPdk?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/5IHbjSKbqMk?controls=0",
          "https://www.youtube-nocookie.com/embed/mhhnO-GeQhI?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/Ku0HxCGXloM?controls=0",
          "https://www.youtube-nocookie.com/embed/CDUw4TK-TKM?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/hJCu-wmgXXc?controls=0",
          "https://www.youtube-nocookie.com/embed/hA42iZ92x2A?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/JZDJY7s4sxo?controls=0",
          "https://www.youtube-nocookie.com/embed/EEfCh0mB2Gw?controls=0",
        ],
      ],
      blueShirtUrls: [
        ["https://www.youtube-nocookie.com/embed/cHICerU0Cl0?controls=0"],
        [
          "https://www.youtube-nocookie.com/embed/pAqdg8xBdfA?controls=0",
          "https://www.youtube-nocookie.com/embed/PHFhB6GQDTk?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/Y3ZKd9WWu08?controls=0",
          "https://www.youtube-nocookie.com/embed/S-qE-VAeWls?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/kL54XFEy19s?controls=0",
          "https://www.youtube-nocookie.com/embed/0TQ5nTvf10Q?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/kCVZvKmcLzk?controls=0",
          "https://www.youtube-nocookie.com/embed/dyRv-YmY1Xc?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/V4Tha3jITpk?controls=0",
          "https://www.youtube-nocookie.com/embed/K1jtPzfQGxw?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/iS4EK8WjAKo?controls=0",
          "https://www.youtube-nocookie.com/embed/cwy55fJbiF0?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/ZuZSupyhS1Q?controls=0",
          "https://www.youtube-nocookie.com/embed/Rgy9DN0L8vU?controls=0",
        ],
        [
          "https://www.youtube-nocookie.com/embed/J85iWruqMRw?controls=0",
          "https://www.youtube-nocookie.com/embed/AfLNxgz567g?controls=0",
        ],
      ],
      q1Answer: null,
      q2Answer: null,
      q3Answer: null,
      q4Answer: null,
      q5Answer: null,
      q6Answer: null,
      q7Answer: null,
      q8Answer: null,
      q9Answer: null,
    };
  },
  methods: {
    nextStep() {
      this.step = this.step + 1;
      this.hasTimerStarted = false;

      this.isVideoComplete = false;
    },
    selectShirt(selection) {
      this.shirtSelection = selection;
      this.q1Answer = selection;

      if (selection === "Blue") {
        this.urls = this.blueShirtUrls;
        this.currentVideoLength = 7500;
      } else if (selection === "White") {
        this.urls = this.whiteShirtUrls;
        this.currentVideoLength = 7500;
      }

      this.intro = false;
      this.hasTimerStarted = false;
      this.isVideoComplete = false;
    },
    recordAnswer(selection, currentStep) {
      if (currentStep === 0) {
        this.q2Answer = selection;
      } else if (currentStep === 1) {
        this.q3Answer = selection;
      } else if (currentStep === 2) {
        this.q4Answer = selection;
      } else if (currentStep === 3) {
        this.q5Answer = selection;
      } else if (currentStep === 4) {
        this.q6Answer = selection;
      } else if (currentStep === 5) {
        this.q7Answer = selection;
      } else if (currentStep === 6) {
        this.q8Answer = selection;
      } else if (currentStep === 7) {
        this.q9Answer = selection;
      }
      this.determineNextVideo(selection, currentStep);
      this.nextStep();
    },
    determineNextVideo(selection, currentStep) {
      if (currentStep === 0) {
        if (selection === "Bar") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 0;
        } else if (selection === "Library") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 1;
        }
      }
      if (currentStep === 1) {
        if (selection === "Ask") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 0;
        } else if (selection === "Tell") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 1;
        }
      }
      if (currentStep === 2) {
        if (selection === "Tell") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 0;
        } else if (selection === "Deflect") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 1;
        }
      }
      if (currentStep === 3) {
        if (selection === "Do") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 0;
        } else if (selection === "Avoid") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 1;
        }
      }
      if (currentStep === 4) {
        if (selection === "Help") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 0;
        } else if (selection === "Talk") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 1;
        }
      }
      if (currentStep === 5) {
        if (selection === "Cover") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 0;
        } else if (selection === "Turn") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 1;
        }
      }

      if (currentStep === 6) {
        if (selection === "Join") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 0;
        } else if (selection === "Let") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 1;
        }
      }

      if (currentStep === 7) {
        if (selection === "Blow") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 0;
        } else if (selection === "Walk") {
          this.videoIndex = this.videoIndex + 1;
          this.videoSubIndex = 1;
        }
        this.sendData();
      }
    },
    sendData() {
      const data = {
        message: `Q1: ${this.q1Answer} Q2: ${this.q2Answer}  Q3: ${this.q3Answer}  Q4: ${this.q4Answer}  Q5: ${this.q5Answer}  Q6: ${this.q6Answer}  Q7: ${this.q7Answer}  Q8: ${this.q8Answer}  Q9: ${this.q9Answer} `,
      };

      send("service_h2kqfs6", "template_cus7mb3", data, "bEaxSBOR7IT2jf03B")
        .then((res) => {
          console.log(res);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    timer(time) {
      if (this.hasTimerStarted === false) {
        this.hasTimerStarted = true;
        console.log("timer started");
        setTimeout(() => {
          this.isVideoComplete = true;
        }, time);
      } else {
      }
    },
    submitDisclaimer() {
      this.disclaimer = false;
      this.mainPhase = true;
      console.log(this.disclaimer);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.video {
  border: 1px solid black;
  width: 400px;
  margin: 20px auto;
  height: 240px;
}

.question {
  width: 400px;
  margin: 20px auto;
}

.option {
  cursor: pointer;
}
</style>
