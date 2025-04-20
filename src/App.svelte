<script lang="ts">
    function copyOutput() {
        const text = document.getElementById("output")!.innerText;
        const elem = document.createElement("textarea");

        document.body.appendChild(elem);
        elem.value = text;

        elem.select();
        elem.setSelectionRange(0, 99999);

        navigator.clipboard.writeText(elem.value);
        document.body.removeChild(elem);
    }

    function generateOutput() {
        // Declare constants
        const leaderScore = parseInt((<HTMLInputElement>document.getElementById("leader")).value);
        const sfPreference = (<HTMLInputElement>document.getElementById("sfpreference")).value;
        const peopleWanted = (<HTMLInputElement>document.getElementById("peoplewanted")).value;
        const numRounds = (<HTMLInputElement>document.getElementById("numrounds")).value;
        const lastUntil = (<HTMLInputElement>document.getElementById("lastuntil")).value;
        const durationType = (<HTMLInputElement>document.getElementById("durationtype")).value;
        const isVeteranRoom = (<HTMLInputElement>document.getElementById("veteran")).checked;
        const roomCode = (<HTMLInputElement>document.getElementById("code")).value;
        const isWaiting = (<HTMLInputElement>document.getElementById("gather")).checked;
        const isStrict = (<HTMLInputElement>document.getElementById("strict")).checked;
        const songPreference = (<HTMLInputElement>document.getElementById("tiersong")).value;

        // Declare variables
        let memberScoreTotal = 0;
        let scoreBoost = 0;
        let desiredScoreBoost = 0;
        let output = document.getElementById("output");

        // Math
        document.querySelectorAll(".pmember").forEach(element => {
            memberScoreTotal += parseInt((<HTMLInputElement>element).value);
        });

        scoreBoost = leaderScore + (memberScoreTotal * 0.2);
        desiredScoreBoost = Math.floor(scoreBoost/10)*10;

        // Sections of the post
        let veteranSection = "";
        if (isVeteranRoom) {
            veteranSection = "ベテラン　";
        }

        let songSection = "";
        switch (songPreference) {
            case "other":
                songSection = "おまかせ";
                break;
            case "shrimp":
                songSection = "🦐";
                break;
            case "lnf":
                songSection = "ロスエン";
                break;
            default:
                break;
        }

        let durationSection = "";
        if (durationType == "numroundstype") {
            if (numRounds == "0") {
                durationSection = "周回";
            } else {
                durationSection = `${numRounds}回`;
            }
        } else {
            durationSection = `${lastUntil}時まで`;
        }

        let peopleWantedSection = `@${peopleWanted}`;

        let roomCodeSection = `🗝️: ${roomCode}`;

        let scoreBoostSection = `主：${scoreBoost}%<br>募集：${desiredScoreBoost}%↑`;

        let sfSection = "";
        switch (sfPreference) {
            case "sfdontcare":
                sfSection = "SF気にしません";
                break;
            case "sfsometimescare":
                sfSection = "SFゆるくミス〇";
                break;
            case "sfcare":
                sfSection = "SFゆるく";
                break;
            default:
                break;
        }

        let waitSection = "";
        if (isWaiting) {
            waitSection = "<br>集まるまで待てる方お願いします";
        }

        let strictSection = "";
        if (isStrict) {
            strictSection = "<br>条件違い解散します";
        }

        // End of post sections

        output!.innerHTML = `${veteranSection}${songSection}${durationSection}<br>${peopleWantedSection}<br><br>${roomCodeSection}<br>${scoreBoostSection}<br><br>${sfSection}${waitSection}${strictSection}<br>#プロセカ募集 #プロセカ協力`
    }

    function changeDurationType() {
        const durationType = document.getElementById("durationtype");
        const numRoundsForm = document.getElementById("numroundsform");
        const lastUntilForm = document.getElementById("lastuntilform");

        if ((<HTMLInputElement>durationType).value == "numroundstype") {
            numRoundsForm!.style.display = "block";
            lastUntilForm!.style.display = "none";
        } else {
            numRoundsForm!.style.display = "none";
            lastUntilForm!.style.display = "block";
        }
    }
</script>

<main>
    <h1>PJSK Co-op Post Builder</h1>

    <p class="credits">Programmed by <a href="https://twitter.com/whimsicalairi" target="_blank">@whimsicalairi</a></p>
    <p class="credits">Massive thanks to <a href="https://twitter.com/starloid_" target="_blank">@starloid_</a> for all the help!</p>

    <hr>

    <div>
        <label for="veteran">Veteran room?</label>
        <input type="checkbox" name="veteran" id="veteran">
    </div>

    <div>
        <label for="tiersong">Pick your preferred song</label>
        <select id="tiersong">
            <option value="other">Other / おまかせ</option>
            <option value="shrimp">Hitorinbo Envy</option>
            <option value="lnf">Lost and Found</option>
        </select>
    </div>

    <div>
        <label for="durationtype">Pick the type of duration you want</label>
        <select id="durationtype" on:change={changeDurationType}>
            <option value="numroundstype"># of rounds</option>
            <option value="lastuntiltype">Last until a certain time</option>
        </select>
    </div>

    <div id="numroundsform">
        <label for="numrounds">How many rounds do you want? (0 for indefinite)</label>
        <input type="text" id="numrounds" value="0">
    </div>

    <div id="lastuntilform">
        <label for="lastuntil">What time do you want it to last to? (JST 24hour)</label>
        <input type="text" id="lastuntil">
    </div>

    <div>
        <label for="peoplewanted">How many people do you want?</label>
        <input type="text" id="peoplewanted" value="4">
    </div>

    <div>
        <label for="code">Enter room code</label>
        <input type="text" id="code">
    </div>

    <p>Enter your score boosts below!</p>
    <div id="team-inputs">
        <input type="text" placeholder="Leader" id="leader">
        <input type="text" class="pmember">
        <input type="text" class="pmember">
        <input type="text" class="pmember">
        <input type="text" class="pmember">
    </div>

    <div>
        <label for="sfpreference">Super fever preference</label>
        <select id="sfpreference">
            <option value="sfdontcare">Don't care about SF</option>
            <option value="sfcare">Aiming for SF</option>
            <option value="sfsometimescare">Aiming for SF but missing sometimes is ok</option>
        </select>
    </div>

    <div>
        <label for="gather">Want people to wait until everyone has gathered?</label>
        <input type="checkbox" name="gather" id="gather">
    </div>

    <div>
        <label for="strict">Strict conditions?</label>
        <input type="checkbox" name="strict" id="strict">
    </div>

    <br>

    <input type="button" value="Submit" on:click="{generateOutput}">

    <p id="output">Output will go here!</p>

    <input type="button" value="Copy to clipboard" on:click="{copyOutput}">
</main>

<style lang="scss">
    @use './styles/style.scss'
</style>
