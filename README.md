-- Variáveis
local frutasDisponiveis = {"Flame", "Magma", "Ice", "Light", "Dark", "Dragon West", "Dragon East", "Yeti", "Kitsune", "leopard", "Gás", "Dough", "T-Rex"}
local comandoPrefixo = "!spawnfruit" -- Prefixo para o comando (com "!" para maior clareza)

-- Função para dar a fruta ao jogador
local function darFrutaAoJogador(player, frutaNome)
    -- Verifica se o nome da fruta existe na lista de frutas disponíveis
    for _, fruta in pairs(frutasDisponiveis) do
        if string.lower(fruta) == string.lower(frutaNome) then
            -- Verifica se a fruta está no ServerStorage
            local frutaObjeto = game.ServerStorage:FindFirstChild(fruta)
            if frutaObjeto then
                -- Clone e coloque a fruta no inventário do jogador
                local cloneFruta = frutaObjeto:Clone()
                local backpack = player:FindFirstChild("Backpack")
                if backpack then
                    cloneFruta.Parent = backpack
                    print(player.Name .. " recebeu a fruta " .. fruta)
                    return
                else
                    print("Backpack não encontrado para o jogador.")
                    return
                end
            else
                player:Kick("A fruta " .. fruta .. " não foi encontrada no ServerStorage.")
                print("Fruta não encontrada no ServerStorage: " .. fruta)
                return
            end
        end
    end
    -- Se a fruta não for válida
    player:Kick("Fruta inválida! Verifique o nome e tente novamente.")
    print("Fruta inválida: " .. frutaNome)
end

-- Função para processar o comando
game.Players.PlayerAdded:Connect(function(player)
    player.Chatted:Connect(function(mensagem)
        -- Verifica se a mensagem começa com o prefixo do comando
        if mensagem:sub(1, #comandoPrefixo) == comandoPrefixo then
            -- Pega o nome da fruta após o comando
            local frutaEscolhida = mensagem:sub(#comandoPrefixo + 2):gsub("^%s*(.-)%s*$", "%1") -- Remove espaços extras
            if frutaEscolhida and frutaEscolhida ~= "" then
                darFrutaAoJogador(player, frutaEscolhida)
            else
                player:Kick("Você precisa digitar o nome da fruta após o comando.")
                print("Nome da fruta não especificado.")
            end
        end
    end)
end)
