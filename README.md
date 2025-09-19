local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local StarterGui = game:GetService("StarterGui")

local function createLoadingScreen()
    local promise = {}
    promise.completed = false

    if CoreGui:FindFirstChild("LoadingScreen") then
        CoreGui.LoadingScreen:Destroy()
    end

    local gui = Instance.new("ScreenGui")
    gui.Name = "LoadingScreen"
    gui.IgnoreGuiInset = true
    gui.ResetOnSpawn = false
    gui.Parent = CoreGui
    pcall(function() syn.protect_gui(gui) end)

    local frame = Instance.new("Frame")
    frame.BackgroundColor3 = Color3.new(0, 0, 0)
    frame.Size = UDim2.new(1, 0, 1, 0)
    frame.Parent = gui

    local star = Instance.new("ImageLabel")
    star.Size = UDim2.new(0, 150, 0, 150)
    star.Position = UDim2.new(0.5, -75, 0.4, -75)
    star.BackgroundTransparency = 1
    star.Image = "rbxassetid://3926307971"
    star.Parent = frame

    local nameText = Instance.new("TextLabel")
    nameText.Size = UDim2.new(1, 0, 0, 50)
    nameText.Position = UDim2.new(0, 0, 0.65, 0)
    nameText.TextColor3 = Color3.fromRGB(255, 255, 255)
    nameText.BackgroundTransparency = 1
    nameText.Font = Enum.Font.GothamBold
    nameText.TextScaled = true
    nameText.Text = "Cookie Hub"
    nameText.Parent = frame

    local particleEmitter = Instance.new("ParticleEmitter")
    particleEmitter.Texture = "rbxassetid://3926307971"
    particleEmitter.Rate = 12
    particleEmitter.Lifetime = NumberRange.new(3, 5)
    particleEmitter.Speed = NumberRange.new(10, 20)
    particleEmitter.Size = NumberSequence.new(0.4, 0.8)
    particleEmitter.Rotation = NumberRange.new(0, 360)
    particleEmitter.RotSpeed = NumberRange.new(-90, 90)
    particleEmitter.Transparency = NumberSequence.new(0, 1)
    particleEmitter.LightEmission = 0.8

    local emitterPart = Instance.new("Part")
    emitterPart.Size = Vector3.new(200, 1, 200)
    emitterPart.Anchored = true
    emitterPart.CanCollide = false
    emitterPart.Transparency = 1
    emitterPart.CFrame = CFrame.new(0, 50, 0)
    emitterPart.Parent = workspace
    particleEmitter.Parent = emitterPart

    task.spawn(function()
        while gui.Parent do
            local t = TweenService:Create(star, TweenInfo.new(1, Enum.EasingStyle.Linear), {Rotation = star.Rotation + 360})
            t:Play()
            t.Completed:Wait()
        end
    end)

    task.delay(5, function()
        TweenService:Create(frame, TweenInfo.new(1), {BackgroundTransparency = 1}):Play()
        TweenService:Create(star, TweenInfo.new(1), {ImageTransparency = 1}):Play()
        TweenService:Create(nameText, TweenInfo.new(1), {TextTransparency = 1}):Play()
        task.wait(1.2)

        StarterGui:SetCore("SendNotification", {
            Title = "Cookie Hub",
            Text = "Bem-vindo ao Cookie Hub!",
            Duration = 3
        })

        promise.completed = true
        gui:Destroy()
        emitterPart:Destroy()
    end)

    return promise
end

local function waitForPromise(promise)
    while not promise.completed do
        task.wait()
    end
end

local loadingPromise = createLoadingScreen()
waitForPromise(loadingPromise)

local redzlib = loadstring(game:HttpGet("https://raw.githubusercontent.com/tbao143/Library-ui/refs/heads/main/Redzhubui"))()
local Window = redzlib:MakeWindow({
    Title = "Cookie Hub",
    SubTitle = "by ixi362 Mat @07x e etc",
    SaveFolder = "CookieHubData"
})

local TabFun = Window:MakeTab({Name = "Fun", Icon = "rbxassetid://3926305904"})
TabFun:AddButton({Name = "Exemplo Botão", Callback = function()
    print("Botão clicado!")
end})

local TabTroll = Window:MakeTab({Name = "Troll", Icon = "rbxassetid://3926305904"})
TabTroll:AddLabel("Área de troll pronta")

local TabMusic = Window:MakeTab({Name = "Music", Icon = "rbxassetid://3926305904"})
TabMusic:AddTextbox({
    Name = "ID da música",
    Default = "",
    TextDisappear = true,
    Callback = function(v)
        local sound = Instance.new("Sound")
        sound.SoundId = "rbxassetid://"..v
        sound.Volume = 3
        sound.Parent = workspace
        sound:Play()
    end
})
