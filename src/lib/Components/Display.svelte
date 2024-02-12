<script>
    export let display = "";
    export let prevDisplay = "";
</script>

<div class="container">
    <div class="prevDisplay">
        <input type="text" disabled bind:value = {prevDisplay}>
    </div>
    <div class="display">
        <input type="text" disabled bind:value = {display}>
    </div>
    <div class="otherBoxes">
        <div class="back-rec-1"><input type="text" disabled></div>
        <div class="back-rec-2"><input type="text" disabled></div>
        <div class="back-rec-3"><input type="text" disabled></div>
    </div>
</div>

<style>
    .otherBoxes {
        position: relative;
        left: -897px;
        top: -169px;
    }

    .container {
        display: flex;
        flex-direction: column;
        align-items: flex-end;
    }

    input {
        color: black;
        box-sizing: border-box;
        border: none;
        border-radius: 5px;
        text-align: right;
        background: none;
    }

    .display {
        z-index: 10;
    }

    .display input {
        width: 225px;
        color: white;
        box-sizing: border-box;
        border: none;
        border-radius: 5px;
        font-size: 35px;
        font-weight: 600;
        margin: -5px 0 15px 0;

        background-image: linear-gradient(120deg, #9deb7f,#118b59);
    }

    .prevDisplay input {
        font-size: 15px;
        width: 100px;
        font-weight: 600;
        background: -webkit-linear-gradient(#76c955,#118b59);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
    }

    .back-rec-1 input {
        width: 205px;
        height: 60px;
        left: 683px;
        top: 102px;
        position: absolute;
        border: none;
        border-radius: 5px;
        background-image: linear-gradient(120deg, #9deb7f,#118b59);
        z-index: 9;
        opacity: 0.7;
    }

    .back-rec-2 input {
        width: 195px;
        height: 45px;
        left: 688px;
        top: 125px;
        position: absolute;
        border: none;
        border-radius: 5px;
        background-image: linear-gradient(120deg, #9deb7f,#118b59);
        z-index: 8;
        opacity: 0.4;
    }

    .back-rec-3 input {
        width: 175px;
        height: 40px;
        left: 698px;
        top: 138px;
        position: absolute;
        border: none;
        border-radius: 5px;
        background-image: linear-gradient(120deg, #9deb7f,#118b59);
        z-index: 7;
        opacity: 0.2;
    }
</style>
