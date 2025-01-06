local Lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/7yhx/kwargs_Ui_Library/main/source.lua"))()

-- Criação da UI
local UI = Lib:Create{
    Theme = "Dark", -- Tema da interface
    Size = UDim2.new(0, 555, 0, 400) -- Tamanho da interface
}

local Main = UI:Tab{
    Name = "Inicia"
}

local Divider = Main:Divider{
    Name = "Funções"
}

local QuitDivider = Main:Divider{
    Name = "Sair"
}

-- Função para pegar todos os trabalhos disponíveis no jogo
local function getJobNames()
    local jobNames = {}
    
    -- Exemplo: Procurar por trabalhos no jogo (nome dos objetos ou locais específicos)
    for _, obj in pairs(workspace:GetChildren()) do
        if obj.Name:find("Job") then  -- Supondo que os trabalhos no jogo tenham "Job" no nome
            table.insert(jobNames, obj.Name)  -- Adiciona o nome à lista
        end
    end
    return jobNames
end

-- Função para teleportar o jogador para um local de trabalho
local function teleportToJob(jobName)
    local jobLocation = workspace:FindFirstChild(jobName)
    if jobLocation then
        local localPlayer = game.Players.LocalPlayer
        local humanoidRootPart = localPlayer.Character:WaitForChild("HumanoidRootPart")
        
        -- Teleportando o jogador para o local do trabalho
        humanoidRootPart.CFrame = jobLocation.CFrame
        print(localPlayer.Name .. " foi teletransportado para " .. jobName)
        
        -- Iniciar o auto-farma imediatamente após teleportar
        autoWork(jobName)
    end
end

-- Função para realizar o trabalho automaticamente (auto-farma)
local function autoWork(jobName)
    local jobLocation = workspace:FindFirstChild(jobName)
    if jobLocation then
        local localPlayer = game.Players.LocalPlayer
        local humanoidRootPart = localPlayer.Character:WaitForChild("HumanoidRootPart")
        
        -- Teleportando o jogador para o local do trabalho (caso ainda não tenha teletransportado)
        humanoidRootPart.CFrame = jobLocation.CFrame

        -- Ação de trabalho automática (por exemplo, coletar recursos)
        while true do
            -- Exemplo: Interagir com uma parte chamada "WorkTask" que representa uma tarefa de trabalho
            local workTask = jobLocation:FindFirstChild("WorkTask") -- Tarefa que o jogador deve realizar
            if workTask then
                -- Simula a interação (ex: coletar ou completar uma tarefa)
                firetouchinterest(humanoidRootPart, workTask, 0)
                firetouchinterest(humanoidRootPart, workTask, 1)
                print(localPlayer.Name .. " está fazendo o trabalho em " .. jobName)
            end
            wait(5)  -- Aguarda 5 segundos antes de repetir a ação
        end
    end
end

-- Função para pegar as armas e equipamentos
local function equipWeapon(weaponName)
    local player = game.Players.LocalPlayer
    local backpack = player.Backpack

    -- Encontrar e equipar a arma
    local weapon = workspace:FindFirstChild(weaponName)
    if weapon then
        -- Pega a arma e coloca na mochila do jogador
        weapon:Clone().Parent = backpack
        print(player.Name .. " equipou " .. weaponName)
    end
end

-- Função para listar todos os trabalhos disponíveis no jogo
local function getJobList()
    local jobNames = getJobNames()
    return jobNames
end

-- Preencher a lista de trabalhos
local jobNamesList = getJobList()

-- Dropdown para escolher um trabalho
local JobDropdown = Divider:Dropdown{
    Name = "Escolha um trabalho",
    Options = jobNamesList, -- Lista de trabalhos no jogo
    Callback = function(jobName)
        -- Quando o trabalho é escolhido, teleportar e começar o trabalho automático
        teleportToJob(jobName)  -- Teleporta para o local de trabalho
    end
}

-- Dropdown para escolher uma arma
local WeaponDropdown = Divider:Dropdown{
    Name = "Escolha uma arma",
    Options = {"Pistola", "Espada", "Rifle"}, -- Exemplo de opções de armas
    Callback = function(weaponName)
        equipWeapon(weaponName)  -- Equipar a arma selecionada
    end
}

-- Botão para fechar a UI
local Quit = QuitDivider:Button{
    Name = "Fechar a biblioteca UI.",
    Callback = function()
        UI:Quit{
            Message = "Saindo...", -- Mensagem ao fechar a interface
            Length = 1 -- Tempo em segundos que a mensagem fica visível
        }
    end
}
