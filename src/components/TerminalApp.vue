<template>
    <div ref="history" v-for="text in texts" @click="onClickHistory">
        {{ text }}
    </div>
    <div id="commandArea">
        $&nbsp;
        <textarea 
            ref="commandAreaInput"
            v-model="command"
            @keydown.enter="onEnter"
            @click.right="onRightClick"
            autofocus 
        />
    </div>
</template>

<script>
    export default {
        updated () {
            this.$refs.history.scrollTop = this.$refs.history.scrollHeight;
            this.commands.time = new Date().toLocaleTimeString();
        },
        data () {
            return {
                texts: [
                    "Welcome to the terminal app\n",
                    "Type 'help' to see the available commands\n",
                    "Created by Orkun Alp Ünlü © 2023\n"
                ],
                command: "",
                commands: {
                    "help": [
                        'help - Displays this message',
                        'clear - Clears the terminal',
                        'whois - Displays information about a user',
                        'ping - Pong!',
                        'time - Tells you the current time',
                        '8ball - Ask the magic 8ball a question',
                        'roll - Roll a dice',
                        'flip - Flip a coin',
                        'stats - Get some statistics about the server',
                        'serverinfo - Get information about the server',
                    ],
                    "whois": "Orkun Alp Ünlü",
                    "ping": "Pong!",
                    "time": new Date().toLocaleTimeString(),
                    "8ball": "Work in Progress...",
                    "roll": "Work in Progress...",
                    "flip": "Work in Progress...",
                    "stats": "Work in Progress...",
                    "serverinfo": "Work in Progress...",
                },
            }
        },
        methods: {
            onEnter (event) {
                event.preventDefault();
                this.texts.push(`$ ${this.command}`);
                this.selectCommand(this.command);
                this.command = "";
            },
            selectCommand (command) {
                if ("clear" === command) {
                    this.texts = [];
                    return;
                }
                if (!this.commands.hasOwnProperty(command)) {
                    this.texts.push("Unknown command");
                    return;
                }
                this.pushTexts(command);
            },
            pushTexts (command) {
                if (Array.isArray(this.commands[command])) {
                    this.texts.push(...this.commands[command]);
                    return;
                }
                this.texts.push(this.commands[command]);
            },
            onRightClick (event) {
                event.preventDefault();
                this.$refs.commandAreaInput.focus();
            },
            onClickHistory(event) {
                event.preventDefault();
                this.$refs.commandAreaInput.focus();
            }
        },
    }
</script>