const { Client, GatewayIntentBits } = require('discord.js');

const client = new Client ({
    instents: [
        GatewayIntentBits.Guilds,
        GatewayIntentBits.GuildVoiceStates,
        GatewayIntentBits.GildMembers
    ]
});

const vocalRoles = {
    "1473075658283290878": "1473314272976044144"
    "1473314165073383538": "1473302288494756046"
    "1473328539255308389": "1473315092673331200"
    "1473328991493554217": "1473315096410325003"
    "1453078273218187308": "1473314803769544766"
    "1473328765173108898": "1473315110415110166"
    "1473328816502870027": "1473315083420565606"
    "1473329266492968970": "1473315104895533298"
    "1473331459682078954": "1473330721820246138"
    "1473331335899779305": "1473316242856349696"
};

client.once('ready', () => {
    console.log('Bot connecté en tant que ${client.user.tag}');
});

client.on('voiceStatepdate', async (oldState, newState) => {
    const member = newState.member;

    try {

        for(const roleId of Object.values(vocalRoles)) {
            if (member.roles.cache.has(roleId)) {
                await member.roles.remove(roleId);
            }
        }

        if (newState.channelId && vocalRoles[newState.channelId]) {
            const roleId = vocalRoles [newState.channelId];
            await member.roles.add(roleId);
            console.log('${member.user.tag} a reçu le rôle pour ${newState.channel.name}');
        }
    } catch (err) {
        console.error(err);
    }
});

client.login("TOKEN");
