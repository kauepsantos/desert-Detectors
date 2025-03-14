-- Criar elementos da interface
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local UICornerMainFrame = Instance.new("UICorner")
local UIStrokeMainFrame = Instance.new("UIStroke")
local Title = Instance.new("TextLabel")
local UICornerTitle = Instance.new("UICorner")
local InfMoneyButton = Instance.new("TextButton")
local UICornerInfMoneyButton = Instance.new("UICorner")
local Message = Instance.new("TextLabel")
local LoadingScreen = Instance.new("Frame")
local LoadingText = Instance.new("TextLabel")

-- Propriedades da ScreenGui
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Propriedades da LoadingScreen
LoadingScreen.Parent = ScreenGui
LoadingScreen.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
LoadingScreen.Size = UDim2.new(1, 0, 1, 0)

-- Propriedades do LoadingText
LoadingText.Parent = LoadingScreen
LoadingText.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
LoadingText.Size = UDim2.new(1, 0, 1, 0)
LoadingText.Font = Enum.Font.SourceSansBold
LoadingText.Text = "Baixando..."
LoadingText.TextColor3 = Color3.fromRGB(255, 255, 255)
LoadingText.TextSize = 40
LoadingText.TextStrokeTransparency = 0
LoadingText.TextWrapped = true

-- Função para simular o carregamento
local function showLoadingScreen()
    LoadingScreen.Visible = true
    wait(3) -- Simular um carregamento de 3 segundos
    LoadingScreen.Visible = false

    -- Criar a interface principal após o carregamento
    -- Propriedades da MainFrame
    MainFrame.Parent = ScreenGui
    MainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    MainFrame.Position = UDim2.new(0.5, -150, 0.5, -125)
    MainFrame.Size = UDim2.new(0, 300, 0, 300)
    MainFrame.Active = true
    MainFrame.Draggable = true

    -- Adicionar cantos arredondados e borda à MainFrame
    UICornerMainFrame.CornerRadius = UDim.new(0, 20)
    UICornerMainFrame.Parent = MainFrame

    UIStrokeMainFrame.Color = Color3.fromRGB(255, 255, 255)
    UIStrokeMainFrame.Thickness = 2
    UIStrokeMainFrame.Parent = MainFrame

    -- Propriedades do Title
    Title.Parent = MainFrame
    Title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    Title.Size = UDim2.new(1, 0, 0, 50)
    Title.Font = Enum.Font.SourceSansBold
    Title.Text = "tubergamer (beta)(1.1)"
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.TextSize = 24

    -- Adicionar cantos arredondados ao Title
    UICornerTitle.CornerRadius = UDim.new(0, 20)
    UICornerTitle.Parent = Title

    -- Propriedades do InfMoneyButton
    InfMoneyButton.Parent = MainFrame
    InfMoneyButton.BackgroundColor3 = Color3.fromRGB(100, 149, 237)
    InfMoneyButton.Position = UDim2.new(0.5, -75, 0.5, -25)
    InfMoneyButton.Size = UDim2.new(0, 150, 0, 50)
    InfMoneyButton.Text = "inf money (beta)"
    InfMoneyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    InfMoneyButton.Font = Enum.Font.SourceSansBold
    InfMoneyButton.TextSize = 20

    -- Adicionar cantos arredondados ao InfMoneyButton
    UICornerInfMoneyButton.CornerRadius = UDim.new(0, 20)
    UICornerInfMoneyButton.Parent = InfMoneyButton

    -- Função para executar o script fornecido
    local function executeInfMoneyScript()
        -- Infinite Power
        local infinite_power do
            local env = getgenv()

            local old; old = hookmetamethod(game, "__namecall", function(self, ...)
                if self.Name == "Change_Power" and string.lower(getnamecallmethod()) == "fireserver" then
                    local v1 = ...
                    if type(v1) == "number" and v1 > 0 then
                        return old(self, math.clamp(env.digging_power or 5, 0, 5555))
                    end
                end
                return old(self, ...)
            end)

            getgenv().digging_power = 9999
        end

        -- Infinite Cash
        local infinite_cash do
            game:GetService('ReplicatedStorage').Purchase_Crate:FireServer(-math.huge)
            task.wait(1)
        end

        -- Unlock All Tools
        local unlock_all_tools do
            local RemoteEvent = game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("RemoteEvent", true)

            if RemoteEvent then
                for _, Tool in ipairs(game:GetService("ReplicatedStorage").Shovels:GetChildren()) do -- Unlock all shovels
                    RemoteEvent:FireServer(Tool.Name)
                end

                for _, Tool in ipairs(game:GetService("ReplicatedStorage").Metal_Detectors:GetChildren()) do -- Unlock all Detectors
                    RemoteEvent:FireServer(Tool.Name)
                end
            end
        end
    end

    -- Conectar o botão InfMoneyButton à função executeInfMoneyScript
    InfMoneyButton.MouseButton1Click:Connect(executeInfMoneyScript)

    -- Propriedades da Message
    Message.Parent = ScreenGui
    Message.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    Message.BackgroundTransparency = 0.5
    Message.Position = UDim2.new(0.5, -100, 0.2, 0)
    Message.Size = UDim2.new(0, 200, 0, 50)
    Message.Font = Enum.Font.SourceSansBold
    Message.Text = ""
    Message.TextColor3 = Color3.fromRGB(255, 255, 255)
    Message.TextSize = 20
    Message.Visible = false
end

-- Função para mostrar mensagem temporária
local function showMessage(text)
    Message.Text = text
    Message.Visible = true
    wait(3) -- Mensagem visível por 3 segundos
    Message.Visible = false
end

-- Mostrar a tela de carregamento
showLoadingScreen()
