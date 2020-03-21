<template>
  <div id="app">
    <Ribbon :url="gitFullUrl"/>

    <Page :divider="false" :fitContent="false" id="main">
      <div class="content noselect">
        <h1 class="title">Broke Protocol: Essentials</h1>
        <h2 class="main-sub">{{github.description}}</h2>
      </div>
    </Page>

    <Page class="info small">
      <div class="info-content">
        <a href="https://www.codacy.com/gh/BPEssentials/Core"><img src="https://api.codacy.com/project/badge/Grade/c3005ebae8e24c38ad8425928b847540"></a>
        <p>// Stargazers: <code>{{github.stargazers_count}}</code> // Watchers: <code>{{github.subscribers_count}}</code> // Open Issues: <code>{{github.open_issues}}</code> // License: <code>{{(github.license || {name: 'Unknown'}).name}}</code></p>
      </div>
    </Page>

    <Page title="# About">
      <MainText>BP:Essentials (also known as BPE) is a plugin used by server owners to make it easier to moderate their server. BPE Also has some commands for players like /kits and /warps, so for everyone there is something you can use. {{commandsLength}} commands defined, and counting. </MainText>
    </Page>

    <Page small title="# Installation">
      <MainText>
        <div>
          <Title xs>Installing the plugin (v3.0+)</Title>
          <p>
            1. Download the latest version from <a :href="gitFullUrl + 'releases/latest'">here</a>. The file should be named "BPEssentials.dll".
            2. Download BP-CoreLib from <a href="https://github.com/UserR00T/BP-CoreLib/releases/latest">here</a>. This is required by BP Essentials. The file should be named "BP-CoreLib.dll".
            3. Download BPE-GameSource from <a href="https://github.com/BPEssentials/GameSource/releases/latest">here</a>. The file should be named "GameSource.dll".
            4. Move all 3 just downloaded files to "YourServerFolder/Plugins". Replace if needed. Once done, start your server and the files should automatically be generated.
          </p>
        </div>
      </MainText>
    </Page>

    <Page small title="# Downloads">
      <MainText>Latest download: <a :href="gitFullUrl + 'releases/latest'">v{{version}}</a>
        View all downloads: <a :href="gitFullUrl + 'releases/'">here</a>
      </MainText>
    </Page>

    <Page small title="# Commands" v-if="settings">
      <MainText>
        <table>
          <tr>
            <th>Name</th>
            <th>Commands</th>
          </tr>
          <tr v-for="(cmd, i) in settings.Commands" :key="i">
            <td>{{cmd.CommandName}}</td>
            <td><code v-for="(c, i) in cmd.Commands" :key="i">{{c}}{{i === cmd.Commands.length - 1 ? '' : ', '}}</code></td>
          </tr>
        </table>
      </MainText>
    </Page>

    <!-- TODO: These two still need to be done 
    <Page small title="# Features">
      <MainText></MainText>
    </Page>

    <Page small title="# FAQ">
      <MainText></MainText>
    </Page> -->
    
    <Page :fitContent="true" class="footer">
      <div>
        <p>Hand crafted with <span style="color: #ff000057;">&lt;3</span> by <a v-for="(developer, i) in developers" :key="i" :href="developer.url">{{developer.name}}{{i === developers.length - 1 ? '' : ', '}}</a>. (c) 2020</p>
      </div>
    </Page>
  </div>
</template>

<script>
import Page from './components/Page';
import MainText from './components/MainText';
import Title from './components/Title';
import Ribbon from './components/Ribbon'

export default {
  name: 'app',
  components: {
    Page,
    MainText,
    Title,
    Ribbon
  },
  data() {
    return {
      version: '3.0',
      git: {
        url: 'https://github.com/',
        urlRaw: 'https://raw.githubusercontent.com/',
        urlApi: 'https://api.github.com/',
        branch: 'master',
        repo: 'BPEssentials/Core/'
      },
      developers: [
        {
          name: 'UserR00T',
          url: 'https://github.com/UserR00T'
        },
        {
          name: 'PLASMAchicken',
          url: 'https://github.com/PLASMAchicken'
        }
      ],
      settings: {},
      github: {}
    }
  },
  methods: {
    fetch(url, options) {
      return fetch(url, options).then(response => response.json());
    },
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    }
  },
  computed: {
    gitFullUrl() {
      return this.git.url + this.git.repo;
    },
    commandsLength() {
      // Due to an async created hook not being awaited in the Vue event loop, we'll need to make an computed property for this.
      if (!this.settings || !this.settings.Commands) {
        return 0;
      }
      return this.settings.Commands.length;
    }
  },
  async created() {
    //this.settings = JSON.parse('{"General":{"AnnounceInterval":120},"Announcements":["test0","test1"],"MessageColors":{},"FunctionUI":{},"ReportOptions":{},"JobIndexArray":{},"WhitelistedJobs":[],"BlockedItems":[],"Misc":{},"Commands":[{"CommandName":"Essentials","Commands":["essentials","ess"]},{"CommandName":"Info","Commands":["info","stats"]},{"CommandName":"OnlinePlayers","Commands":["players","online"]},{"CommandName":"Atm","Commands":["atm","bank","bal","balance"]},{"CommandName":"Feed","Commands":["feed","maxstats","ms"]},{"CommandName":"Heal","Commands":["heal","hp"]},{"CommandName":"Save","Commands":["save","savegame"]},{"CommandName":"Free","Commands":["free"]},{"CommandName":"Kill","Commands":["kill"]},{"CommandName":"Arrest","Commands":["arrest"]},{"CommandName":"Restrain","Commands":["restrain"]},{"CommandName":"ClearWanted","Commands":["clearwanted","cw"]},{"CommandName":"Launch","Commands":["launch","takeoff"]},{"CommandName":"Slap","Commands":["slap","slp"]},{"CommandName":"Knockout","Commands":["knockout","ko"]},{"CommandName":"ToggleChat","Commands":["t","tchat"]},{"CommandName":"Promote","Commands":["promote","rankup"]},{"CommandName":"Disconnect","Commands":["disconnect","dconnect"]},{"CommandName":"PmUser","Commands":["pm","pmuser"]},{"CommandName":"ReplyUser","Commands":["r","ruser","replyuser"]},{"CommandName":"TpaUser","Commands":["tpa","tprequest","teleportrequest"]},{"CommandName":"TpaAccept","Commands":["tpaccept","tpaaccept","tpaa","accept"]},{"CommandName":"TpaDeny","Commands":["tpdeny","tpadeny","tpad","deny"]},{"CommandName":"Back","Commands":["back","return"]},{"CommandName":"Teleport","Commands":["tp"]},{"CommandName":"TeleportHere","Commands":["tph","tphere"]}]}');
    this.settings = await this.fetch(`${this.git.urlRaw}${this.git.repo}${this.git.branch}/dist/settings.json`);
    this.github = await this.fetch(`${this.git.urlApi}repos/${this.git.repo.slice(0, -1)}`);

    // It's the little things that count :)
    this.shuffleArray(this.developers);  
  },
  mounted() {
    VANTA.WAVES({
      el: "#main",
      color: '#751118',
      shininess: 13.00,
      waveHeight: 12.50,
      waveSpeed: 1.20,
      zoom: 0.86
    })
  }
}
</script>

<style lang="scss">
@import "@/scss/global.scss";

ul {
  padding-left: 0;
  list-style-type: none;
  &.contact li {
    display: inline;
  }
}

::-webkit-scrollbar {
  width: map-get($scrollbar, width);
  height: map-get($scrollbar, height);
}
::-webkit-scrollbar-thumb {
  background: map-get($scrollbar, thumb);
}
::-webkit-scrollbar-track {
  background: map-get($scrollbar, track);
}

* {
  color: map-get($colors, text-default);
}
a[href] {
  text-decoration: none;
  color: map-get($colors, href);
}
p {
  white-space: pre-line;
}


table {
  margin: 0 5% 0 0;
  width: 100%;
  th {
    text-align: left;
    background-color: map-get($colors, table-header);
  }
  td, th {
    padding: 8px;
  }
  td {
    border-bottom: 1px solid map-get($colors, table-border);
    @media (max-width: 780px) {   
      font-size: 0.8em;
    }
  }
  > tr:last-child > td {
    border-bottom: 0;
  }
  td:nth-child(1) {
    max-width: 2vw;
  }
  td:nth-child(2) {
    max-width: 15vw;
  }
  tr:hover {
    background-color: darken(map-get($colors, background), 1%);
  }
}

.info {
  .info-content {
    display: flex;
    padding: 0 5%;

    >* {
      margin: 7px 0 5px;
    }
    p {
      padding: 2px 20px;

      code {
        background-color: darken(map-get($colors, background), 1%);
        border-radius: 3px;
        font-family: courier, monospace;
        padding: 0 3px;
      }
    }
  }
}

.footer, .small {
  .fit-content {
    padding-bottom: 0;
  }
  p {
    text-align: center;
    color: map-get($colors, footer-text);
  }
}

.main-sub {
  font-family: map-get($fonts, main-sub);
  font-size: 2em;
  font-weight: 100;
}

.content {
  text-align: center;
  margin: 0 25%;
  padding: 0 0 3vh 0;
}

.noselect {
  -webkit-touch-callout: none;
    -webkit-user-select: none;
     -khtml-user-select: none;
       -moz-user-select: none;
        -ms-user-select: none;
            user-select: none;
}

body {
  overflow-x: hidden;
  background-color: map-get($colors, background);
}

html, body {
  margin: 0;
  padding: 0;
}
#main {
  padding: 20vh 0 0;
}

#app {
  font-family: map-get($fonts, main);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
