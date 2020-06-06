<template>
    <div id="app">
        <div class="flex">
            <div class="min-h-screen left-0 flixed w-2/12 text-white" id="sideBar"></div>
            <div class="min-h-screen left-0 flixed w-10/12 bg-black text-white">
                <p class="text-3xl mb-20 font-semibold text-center mt-20">Welcome to the MelloSo. Player your desired music here.</p>
            </div>
        </div>
        <audio ref="aud" id="ai">
            <source :src="songSource[0]" type="audio/mpeg">
        </audio>

        <div class="flex bottom-0 fixed text-white  w-full h-20 items-center" id="player">

            <div class="w-1/3 text-center">
                <div class="flex items-center">
                    <img src="assets/img/Cover.jpg" class="h-20 w-auto" alt="">
                    <div>
                        <h1 class="font-light text-lg mx-3 text-left">{{ songName }}</h1>
                        <h1 class="font-semibold text-sm mx-3">{{ songProducer }}</h1>
                    </div>
                </div>
            </div>

            <div class="w-1/3">

                <div class="flex flex-col text-center">
                    <div id="buttons" class="mb-3">
                        <button v-if="!play" @click="play=true,playSong()" class=" focus:outline-none" title="Play">
                            <svg aria-hidden="true" focusable="false" data-icon="play" role="img"
                                 xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" class="h-4 w-4 text-white">
                                <path fill="currentColor"
                                      d="M424.4 214.7L72.4 6.6C43.8-10.3 0 6.1 0 47.9V464c0 37.5 40.7 60.1 72.4 41.3l352-208c31.4-18.5 31.5-64.1 0-82.6zm-16.2 55.1l-352 208C45.6 483.9 32 476.6 32 464V47.9c0-16.3 16.4-18.4 24.1-13.8l352 208.1c10.5 6.2 10.5 21.4.1 27.6z"
                                      class=""></path>
                            </svg>
                        </button>
                        <button v-if="play" @click="play=false,pauseSong()" class=" focus:outline-none" title="Pause">
                            <svg aria-hidden="true" focusable="false" data-icon="pause-circle" role="img"
                                 xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="h-4 w-4 text-white">
                                <path fill="currentColor"
                                      d="M218 160h-20c-3.3 0-6 2.7-6 6v180c0 3.3 2.7 6 6 6h20c3.3 0 6-2.7 6-6V166c0-3.3-2.7-6-6-6zm96 0h-20c-3.3 0-6 2.7-6 6v180c0 3.3 2.7 6 6 6h20c3.3 0 6-2.7 6-6V166c0-3.3-2.7-6-6-6zM256 8C119 8 8 119 8 256s111 248 248 248 248-111 248-248S393 8 256 8zm0 464c-118.7 0-216-96.1-216-216 0-118.7 96.1-216 216-216 118.7 0 216 96.1 216 216 0 118.7-96.1 216-216 216z"
                                      class=""></path>
                            </svg>
                        </button>
                        <button @click="play=false,stopSong()" class="ml-5 focus:outline-none" title="Stop">
                            <svg aria-hidden="true" focusable="false" data-prefix="far" data-icon="stop" role="img"
                                 xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" class="h-4 w-4 text-white">
                                <path fill="currentColor"
                                      d="M400 32H48C21.5 32 0 53.5 0 80v352c0 26.5 21.5 48 48 48h352c26.5 0 48-21.5 48-48V80c0-26.5-21.5-48-48-48zm-6 400H54c-3.3 0-6-2.7-6-6V86c0-3.3 2.7-6 6-6h340c3.3 0 6 2.7 6 6v340c0 3.3-2.7 6-6 6z"
                                      class=""></path>
                            </svg>
                        </button>
                    </div>

                    <div id="seeker" class="inline-flex mx-auto items-center">
                        <div class="mx-3">{{ startTime }}</div>
                        <input type="range" min="1" max="100" ref="songSeek" id="si" @click="randomF"

                               class="cursor-pointer w-64 h-px slider">
                        <div class="mx-3">{{ dur }}</div>
                    </div>
                </div>

            </div>

            <div class="w-1/3 justify-end items-center flex">
                <div class="mt-1">
                    <button v-if="!mute" class="mr-3" @click="mute=true,muteVolume()">
                        <svg aria-hidden="true" focusable="false" data-prefix="fal" data-icon="volume-up" role="img"
                             xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512"
                             class="h-5 w-5">
                            <path fill="currentColor"
                                  d="M342.91 193.57c-7.81-3.8-17.5-.48-21.34 7.5-3.81 7.97-.44 17.53 7.53 21.34C343.22 229.2 352 242.06 352 256s-8.78 26.8-22.9 33.58c-7.97 3.81-11.34 13.38-7.53 21.34 3.86 8.05 13.54 11.29 21.34 7.5C368.25 306.28 384 282.36 384 256s-15.75-50.29-41.09-62.43zM231.81 64c-5.91 0-11.92 2.18-16.78 7.05L126.06 160H24c-13.26 0-24 10.74-24 24v144c0 13.25 10.74 24 24 24h102.06l88.97 88.95c4.87 4.87 10.88 7.05 16.78 7.05 12.33 0 24.19-9.52 24.19-24.02V88.02C256 73.51 244.13 64 231.81 64zM224 404.67L139.31 320H32V192h107.31L224 107.33v297.34zM421.51 1.83c-7.89-4.08-17.53-1.12-21.66 6.7-4.13 7.81-1.13 17.5 6.7 21.61 84.76 44.55 137.4 131.1 137.4 225.85s-52.64 181.3-137.4 225.85c-7.82 4.11-10.83 13.8-6.7 21.61 4.1 7.75 13.68 10.84 21.66 6.7C516.78 460.06 576 362.67 576 255.99c0-106.67-59.22-204.06-154.49-254.16zM480 255.99c0-66.12-34.02-126.62-88.81-157.87-7.69-4.38-17.59-1.78-22.04 5.89-4.45 7.66-1.77 17.44 5.96 21.86 44.77 25.55 72.61 75.4 72.61 130.12s-27.84 104.58-72.61 130.12c-7.72 4.42-10.4 14.2-5.96 21.86 4.3 7.38 14.06 10.44 22.04 5.89C445.98 382.62 480 322.12 480 255.99z"
                                  class=""></path>
                        </svg>
                    </button>

                    <button v-if="mute" class="mr-3" @click="mute=false,muteVolume()">
                        <svg aria-hidden="true" focusable="false" data-prefix="fal" data-icon="volume-slash" role="img"
                             xmlns="http://www.w3.org/2000/svg" viewBox="0 0 640 512"
                             class="h-5 w-5">
                            <path fill="currentColor"
                                  d="M256 107.3v37.2l32 25.2V88c0-14.5-11.9-24-24.2-24-5.9 0-11.9 2.2-16.8 7l-37.1 37.1 25.3 19.9zm152 18.6c44.4 25.5 72 75.4 72 130.1 0 19.8-3.7 39-10.5 56.6l25.9 20.4c10.6-23.6 16.6-49.8 16.6-77 0-66.1-33.7-126.6-88-157.9-7.6-4.4-17.4-1.8-21.8 5.9-4.5 7.7-1.8 17.5 5.8 21.9zm30.8-95.7C523.4 74.7 576 161.2 576 256c0 41.8-10.7 81.7-29.4 117.3l25.5 20.1C595 352 608 305.1 608 256c0-106.7-59.1-204.1-154.2-254.2-7.9-4.1-17.5-1.1-21.6 6.7-4.2 7.9-1.2 17.5 6.6 21.7zm-77.7 192.2c9.5 4.5 16.3 12 19.9 20.5l33.1 26c.9-4.3 1.9-8.5 1.9-13 0-26.4-15.8-50.3-41.1-62.4-7.8-3.8-17.5-.5-21.3 7.5s-.5 17.6 7.5 21.4zM637 485.2L23 1.8C19.6-1 14.5-.5 11.8 3l-10 12.5C-1 19-.4 24 3 26.7l614 483.5c3.4 2.8 8.5 2.2 11.2-1.2l10-12.5c2.8-3.5 2.3-8.5-1.2-11.3zm-198.2-3.4c-7.8 4.1-10.8 13.8-6.7 21.6 2.8 5.3 8.2 8.6 14.2 8.5 2.5 0 5.1-.6 7.5-1.8 21.7-11.4 41.2-25.5 58.8-41.4l-25.7-20.2c-14.7 12.6-30.5 24.1-48.1 33.3zM256 404.7L171.3 320H64V192h97.1l-40.6-32H56c-13.3 0-24 10.7-24 24v144c0 13.2 10.7 24 24 24h102.1l89 89c4.9 4.9 10.9 7 16.8 7 12.3 0 24.2-9.5 24.2-24V291.9l-32-25.2v138z"
                                  class=""></path>
                        </svg>
                    </button>
                </div>
                <input type="range" min="1" max="100" ref="songVol" @click="changeVolume"
                       class="cursor-pointer w-40 mr-10 h-1 slider" id="myRange">
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                songName: "Thunder",
                songProducer: "Imagine Dragons",
                play: false,
                mute: false,
                vol: " ",
                startTime: " ",
                endTime: " ",
                durationTotal: 0,
                songSource: [
                    "assets/songs/01. Thunder.mp3",
                    "assets/songs/Anne-Marie - 2002.mp3",
                ]
            }
        },
        methods: {
            convertTimeHHMMSS(){
                let au = this.$refs["aud"];
                au.onloadedmetadata = function() {
                    console.log(au.duration)
                };
                let val = " "
                // console.log(val)
                let hhmmss = new Date(val * 1000).toISOString().substr(11, 8);
                return hhmmss.indexOf("00:") === 0 ? hhmmss.substr(3) : hhmmss;
            },
            playSong() {
                this.$refs["aud"].play();
                // let x = ;
                console.log(this.$refs["aud"].duration);
                window.setInterval(() => {
                    if (this.play) {
                        this.songSeeked();
                    }
                }, Math.floor(this.$refs["aud"].duration) * 10)

            },

            pauseSong() {
                this.$refs["aud"].pause();
            },

            stopSong() {
                this.$refs["aud"].pause();
                this.$refs["aud"].currentTime = 0;
                this.$refs["songSeek"].value = 0;
            },
            changeVolume() {
                this.$refs["aud"].volume = this.$refs["songVol"].value / 100;
            },
            muteVolume() {
                if (!this.mute) {
                    this.$refs["aud"].volume = this.vol;
                    this.$refs["songVol"].value = this.vol * 100;
                } else if (this.mute) {
                    this.vol = this.$refs["aud"].volume;
                    this.$refs["aud"].volume = 0;
                    this.$refs["songVol"].value = 0;
                }
            },
            randomF() {
                this.$refs["aud"].currentTime = (this.$refs["songSeek"].value / 100) * this.$refs["aud"].duration;
                console.log(this.$refs["aud"].currentTime);
            },
            songSeeked() {
                this.$refs["songSeek"].value++;
                console.log(Math.floor(this.$refs["aud"].duration) * 10)
            },
        },
        computed: {
            dur(){
                return this.convertTimeHHMMSS();
            }
        },
        mounted() {
            this.$refs["aud"].volume = 0.5
            this.$refs["songVol"].value = this.$refs["aud"].volume * 100
            this.$refs["songSeek"].value = 0;
            this.seekedValue = this.$refs["aud"].currentTime
            // document.getElementById("ai").onloadedmetadata = function() {
            //     this.durationTotal = convertTimeHHMMSS(parseInt(document.getElementById('ai').duration));
            //     console.log(this.durationTotal)
            // };
        },
    }


</script>
<style>
    button:focus {
        outline: none;
    }
    input:focus {
        outline: none;
    }

    #sideBar {
        background: rgba(0, 0, 0, 0.79);
    }

    #player {
        background: #3C95AF;
    }

    .slider::-moz-range-thumb {
        width: 12px;
        height: 12px;
        border: 2px solid white;
        border-radius: 50%;
        background: #3C95AF;
        cursor: pointer;
    }
</style>
