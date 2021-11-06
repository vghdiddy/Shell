// ==UserScript==
// @name         Toggleable (Key V) ESP for Shell Shockers (PROOF OF CONCEPT)
// @version      0.1
// @author       A3+++
// @description  Only to be used for TESTING purposes, in private lobbies.
// @match        https://shellshock.io/*
// @grant        none
// @namespace    https://greasyfork.org/users/815159
// @run-at       document-start
// ==/UserScript==

(function () {
    "use strict"
    window.espEnabled = false;
    window.espKey = "V";
    window.addEventListener("keydown", function (e) {
        if (extern.inGame) {
            if (e.key.toUpperCase() == window.espKey) {
                window.espEnabled = !window.espEnabled
            }
        } else {
            window.espEnabled = false
        }
    })
    XMLHttpRequest = class extends XMLHttpRequest {
        constructor() {
            super(...arguments)
        }
        open() {
            if (arguments[1] && arguments[1].includes("src/shellshock.js")) {
                this.scriptMatch = true;
            }

            super.open(...arguments)
        }
        get response() {
            if (this.scriptMatch) {
                let responseText = super.response;

                let i = 0;
                responseText = responseText.replace(/.prototype.setVisible=function\(\w\){/g, match => ++i == 2 ? `.prototype.setVisible=function(${match.match(/function\((\w)\){/)[1]}){${match.match(/function\((\w)\){/)[1]}=true;this.getChildTransformNodes().forEach(child=>child.setRenderingGroupId&&child.setRenderingGroupId(window.espEnabled?1:0));`:match);

                return responseText;
            }
            return super.response;
        }
    };
}())
