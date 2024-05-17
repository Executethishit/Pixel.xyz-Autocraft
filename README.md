# AutoCraft Script

## Description

AutoCraft is a userscript designed to automate crafting actions in the game Pixels. It automatically crafts an item and collects it with random delays to avoid bans.

## Installation

1. Install a userscript manager extension like [Tampermonkey](https://www.tampermonkey.net/) for your browser.
2. Click on the Tampermonkey icon and select "Create a new script".
3. Copy the contents of the `AutoCraft by trojan10-0.1.user.js` file into the new script.
4. Save the script.

## Usage

The script automates the following actions:

- Detects if the "Create" button is clickable and clicks it with a random delay.
- Checks for an energy popup and closes it if energy is insufficient.
- Detects if the "Collect" button is clickable and clicks it with a random delay.

## Script Details

```javascript
// ==UserScript==
// @name         AutoCraft by trojan10
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Automatically crafts an item and collects it in a game with random delays to avoid ban. No more unnecessary clicks or popups!
// @author       Trojan
// @match        https://play.pixels.xyz/
// @icon         https://www.google.com/s2/favicons?sz=64&domain=pixels.tips
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    function getRandomDelay() {
        return Math.random() * 10;
    }

    function isCreateButtonClickable() {
        var createButton = document.querySelector('button.Crafting_craftingButton__Qd6Ke:not([disabled]) span');
        return createButton !== null && createButton.textContent === "Create";
    }

    function isCollectButtonClickable() {
        var collectButton = document.querySelector('button.Crafting_craftingButton__Qd6Ke:not([disabled]) span');
        return collectButton !== null && collectButton.textContent === "Collect";
    }

    function clickCreateButton() {
        var createButton = document.querySelector('button.Crafting_craftingButton__Qd6Ke span');
        if (createButton) {
            createButton.click();
            setTimeout(checkEnergyPopup, 1000); // Wait for 1 second before checking for energy popup
        }
    }

    function checkEnergyPopup() {
        var energyPopup = document.querySelector('.Notifications_text__ak1FH');
        if (energyPopup && energyPopup.textContent === "Not enough energy") {
            var closeButton = document.querySelector('button.Crafting_craftingCloseButton__ZbHQF');
            if (closeButton) {
                closeButton.click();
            }
        }
    }

    function clickCollectButton() {
        var collectButton = document.querySelector('button.Crafting_craftingButton__Qd6Ke span');
        if (collectButton) {
            collectButton.click();
        }
    }

    function performActions() {
        if (isCreateButtonClickable()) {
            var createDelay = getRandomDelay();
            console.log("Waiting " + createDelay + " milliseconds before clicking Create button.");
            setTimeout(clickCreateButton, createDelay);
        } else if (isCollectButtonClickable()) {
            var collectDelay = getRandomDelay();
            console.log("Waiting " + collectDelay + " milliseconds before clicking Collect button.");
            setTimeout(clickCollectButton, collectDelay);
        }
    }

    setInterval(performActions, 0);

    console.log("Script loaded! For support, join our Discord: [https://discord.gg/7TSJKtUK6Q]");
})();
