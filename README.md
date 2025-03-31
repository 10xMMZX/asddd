local DhLibrary = loadstring(game:HttpGet("https://raw.githubusercontent.com/penguin-cmyk/legion/refs/heads/main/utils/dh_library.lua"))()

local Services   = DhLibrary:Services()

local LightingService   = game:GetService("Lighting")
local ReplicatedStorage = Services.ReplicatedStorage
local LocalPlayer       = Services.LocalPlayer
local RunService        = Services.RunService
local Workspace         = Services.Workspace
local Players           = Services.Players 
------------------------------------------------------------------------------------------------------------

local HollowPurple = game:GetObjects("rbxassetid://113021961115483")[1]
local RoadRoller = game:GetObjects("rbxassetid://85904556439762")[1]
local SonicRings = game:GetObjects("rbxassetid://107016840921635")[1]
local BlackHole  = game:GetObjects("rbxassetid://107558636253769")[1]
local Spirit     = game:GetObjects("rbxassetid://89731535148122")[1]
local Charm      = game:GetObjects("rbxassetid://139067309930404")[1]
local Thanos     = game:GetObjects("rbxassetid://129891025243944")[1]
local AfterSlash = game:GetObjects("rbxassetid://102168430997910")[1]
local Rune       = game:GetObjects("rbxassetid://139139192547301")[1]

local TweenService = game:GetService("TweenService")
local AssetService = game:GetService("AssetService")

local Train = AssetService:CreateMeshPartAsync("rbxassetid://743576537")
local Aura = AssetService:CreateMeshPartAsync("http://www.roblox.com/asset/?id=1323306")

local Camera  = Workspace.CurrentCamera 

-- For Rune
local Shock = AssetService:CreateMeshPartAsync("rbxassetid://5139788115")
local Wind  = AssetService:CreateMeshPartAsync("rbxassetid://4681227436")
local Root  = AssetService:CreateMeshPartAsync("rbxassetid://5139788213")

Shock.Color = Color3.fromRGB(248, 248, 248)
Shock.Anchored = true 
Shock.Parent = workspace
Shock.CastShadow = false 
Shock.CanCollide = false 
Shock.Material = "Neon"

Wind.Color = Color3.fromRGB(248, 248, 248)
Wind.Anchored = true 
Wind.Parent = workspace
Wind.CastShadow = false 
Wind.CanCollide = false 
Wind.Material = "Neon"

Root.Color = Color3.fromRGB(248, 248, 248)
Root.Anchored = true 
Root.Parent = workspace
Root.CastShadow = false 
Root.CanCollide = false 
Root.Material = "Neon"



Aura.Color = Color3.fromRGB(4, 175, 236)
Aura.Anchored = true 
Aura.Parent = workspace
Aura.CanCollide = false 
Aura.CastShadow = false 

Train.TextureID = "rbxassetid://743576568"
Train.Anchored = true 
Train.Parent = workspace
Train.CFrame = Train.CFrame * CFrame.Angles(0,-math.rad(90),0)
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------
function PlaySound(ID: number, Vol: number, StartTime: number?)
    local sound = Instance.new("Sound", workspace)
    sound.SoundId = "rbxassetid://"..ID
    sound.Volume = Vol or 1
    if StartTime then
        sound.TimePosition = StartTime
    end
    sound:Play()
    return sound
end

function StopSound(sound: any)
    if sound and sound.IsPlaying then
        sound:Stop()
        sound:Destroy()
    end
end

local ShakeModule = {}

local shakeActive = false
local shakeStartTime = 0
local shakeDuration = 0.8
local initialIntensity = 1

local noise = math.noise 

local function updateCameraShake()
    if not shakeActive then return end
    
    local elapsed = os.clock() - shakeStartTime
    if elapsed >= shakeDuration then
        shakeActive = false
        return
    end
    
    local progress = elapsed / shakeDuration
    local currentIntensity = initialIntensity * (1 - progress^2)
    
    local downwardOffset = Vector3.new(0, -0.5 * currentIntensity, 0)
    local bounceOffset = Vector3.new(0, 0.3 * currentIntensity * math.sin(progress * 10), 0)
    
    local timeFactor = os.clock() * 10
    local shakeX = noise(timeFactor, 0) * currentIntensity * 0.1
    local shakeY = noise(timeFactor, 100) * currentIntensity * 0.1
    local shakeZ = noise(timeFactor, 200) * currentIntensity * 0.1
    
    local rotX = noise(timeFactor, 300) * currentIntensity * 0.05 
    local rotY = noise(timeFactor, 400) * currentIntensity * 0.05 
    local rotZ = noise(timeFactor, 500) * currentIntensity * 0.05 
    
    local offsetPosition = downwardOffset + bounceOffset + Vector3.new(shakeX, shakeY, shakeZ)
    local offsetRotation = CFrame.Angles(rotX, rotY, rotZ)
    
    Camera.CFrame = Camera.CFrame * CFrame.new(offsetPosition) * offsetRotation
end

function ShakeModule.StartShake(intensity, duration)
    initialIntensity = intensity or 1
    shakeDuration = duration or 0.8
    shakeStartTime = os.clock()
    shakeActive = true
end

DhLibrary:AddConnection("ShakeModule", RunService.Heartbeat:Connect(updateCameraShake))
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
local StompModule = {
    StompEffect = "HollowPurple",
    StompCooldown = 2,
    lastStompTime = 0,
    CurrentEffects = {}
}
StompModule.StartShake = ShakeModule.StartShake

local Effects = {
    ["RoadRoller"] = function(targetCharacter)
        RoadRoller.Parent = workspace
        RoadRoller.Anchored = true 
        RoadRoller.CFrame = targetCharacter.HumanoidRootPart.CFrame * CFrame.new(0, 40, 0)
        local Sound = Instance.new("Sound",workspace)
        Sound.SoundId = "rbxassetid://4877348549"
        Sound.PlayOnRemove = true 
        Sound:Destroy()    
        
        TweenService:Create(
            RoadRoller,
            TweenInfo.new(0.2), 
            { 
                CFrame = targetCharacter.HumanoidRootPart.CFrame * 
                        CFrame.new(0, 3, 0) * 
                        CFrame.Angles(math.rad(200), -math.rad(200), math.rad(90)) 
            }
        ):Play()

        ShakeModule.StartShake(7, 0.5)

        task.wait(2)
        RoadRoller.CFrame = CFrame.new(20,2000,20)
    end,

    ["Rings"] = function(targetCharacter) 
        local Sound = Instance.new("Sound",workspace)
        Sound.SoundId = "rbxassetid://7456436580"
        Sound.PlayOnRemove = true 
        Sound:Destroy()

        local FakePart = Instance.new("Part",workspace)
        FakePart.Transparency = 1
        FakePart.Position = targetCharacter.Head.Position
        FakePart.Anchored = true 
        FakePart.CanCollide = false 
    
        SonicRings.Parent = FakePart
        task.wait(0.5)
        SonicRings.Parent = workspace
        FakePart:Destroy()
    end,

    ["Calaypse"] = function(targetCharacter)
        function calaypse(startpos, ranpos, color, brightness, textureLength, textureSpeed, zOffset, width0, width1, segments)
            local beam = Instance.new("Beam", workspace)
            beam.Texture = "rbxassetid://7208353354"
            beam.Enabled = true
            beam.Brightness = brightness
            beam.LightEmission = 1
            beam.LightInfluence = 0
            beam.TextureLength = textureLength
            beam.TextureSpeed = textureSpeed
            beam.ZOffset = zOffset
            beam.Segments = segments
            beam.Width0 = width0
            beam.Width1 = width1
            beam.Color = ColorSequence.new(color)
        
            local attachment0 = Instance.new("Attachment", workspace.Terrain)
            local attachment1 = Instance.new("Attachment", workspace.Terrain)
            local portal = game:GetObjects("rbxassetid://80500412634796")[1]
            local explosion = game:GetObjects("rbxassetid://72690882247135")[1]
            
            attachment0.Position = startpos
            attachment1.Position = ranpos
            attachment1.CFrame = CFrame.new(attachment1.Position, attachment1.Position + (attachment1.Position - attachment0.Position).unit)
            beam.Attachment0 = attachment0
            beam.Attachment1 = attachment1
            portal.Position = attachment1.Position
            ShakeModule.StartShake(2, 0.5)
            for i, v in portal:GetChildren() do
                v.Parent = attachment1
                v.EmissionDirection = Enum.NormalId.Front
            end
            for i, v in explosion:GetChildren() do
                v.Parent = attachment0
            end
            explosion.Position = attachment0.Position
            PlaySound(76786040776528)
            PlaySound(180204650)
            TweenService:Create(beam, TweenInfo.new(0.25), {Width0 = beam.Width0,Width1 = beam.Width1}):Play()
            local laster = PlaySound(6290067239)
        
            task.delay(0.5, function()
                TweenService:Create(beam, TweenInfo.new(0.5), {Width0 = 0,Width1 = 0}):Play()
                delay(0.5, function()
                    game:GetService("Debris"):AddItem(beam, 0.5)
                    game:GetService("Debris"):AddItem(attachment1, 0)
                    game:GetService("Debris"):AddItem(attachment0, 0)
                    StopSound(laster)
                end)
            end)
        
            return beam
        end
        
        for i = 1, 3 do
            calaypse(targetCharacter.UpperTorso.Position, targetCharacter.UpperTorso.Position + Vector3.new(math.random(-44, 44), math.random(20, 44), math.random(-44, 44)), Color3.fromRGB(234, 152, 255), 4, 1, 3, -0.2, 12, 28, 5)
            calaypse(targetCharacter.UpperTorso.Position, targetCharacter.UpperTorso.Position + Vector3.new(math.random(-44, 44), math.random(20, 44), math.random(-44, 44)), Color3.fromRGB(234, 152, 255), 4, 1, 3, -0.2, 12, 28, 5)
            calaypse(targetCharacter.UpperTorso.Position, targetCharacter.UpperTorso.Position + Vector3.new(math.random(-44, 44), math.random(20, 44), math.random(-44, 44)), Color3.fromRGB(241, 190, 255), 2, 0.2, 1, -0.1, 8, 25, 5)
            task.wait(.2)
        end
        for _, part in pairs(targetCharacter:GetDescendants()) do 
            if part:IsA("BasePart") or part:IsA("MeshPart") then 
                TweenService:Create(part,TweenInfo.new(0.5),{
                    Transparency = 1
                }):Play()
            end
        end
    end,

    ["Meteor"] = function(targetCharacter)
        local meteor = game:GetObjects("rbxassetid://131480293477477")[1]
        local explosion = game:GetObjects("rbxassetid://83623440887243")[1]
        local at = Instance.new("Attachment",workspace.Terrain)
        local at2 = Instance.new("Attachment",workspace.Terrain)
        
        local burn = PlaySound(8533770574,1,.4)
        at.Position = targetCharacter.UpperTorso.Position + Vector3.new(math.random(-115, 115), math.random(150, 300), math.random(-115, 115))
        at2.Position = targetCharacter.UpperTorso.Position
        
        for i,v in meteor:GetChildren() do
            v.Parent = at
            v.Enabled = true
        end
        
        TweenService:Create(at, TweenInfo.new(1, Enum.EasingStyle.Linear, Enum.EasingDirection.Out), {
        	Position = targetCharacter.UpperTorso.Position;
        }):Play()
        
        burn.Ended:Connect(function()
            PlaySound(577577319)
            for i, v in pairs(targetCharacter:GetChildren()) do
                if v:IsA("BasePart") and v.Name ~= "HumanoidRootPart" then
                    v.Color = Color3.fromRGB(0, 0, 0)
                    local fire = Instance.new("Fire")
                    fire.Size = 10
                    fire.Heat = 50
                    fire.Parent = v
                end
            end
            for i,v in explosion:GetChildren() do
                v.Parent = at2
                v.Enabled = true
            end
        end)

        task.wait(3.4)
        game:GetService("Debris"):AddItem(at,.1)
        game:GetService("Debris"):AddItem(at2,.1)
        game:GetService("Debris"):AddItem(fire,1)
    end,

    ["Crystal"] = function(targetCharacter)
        for i = 1, 3 do
            task.delay(i * 0.3, function()
                local object = game:GetObjects("rbxassetid://125115196741368")[1]
                local wisp = game:GetObjects("rbxassetid://139168481251179")[1]
                local explosion = game:GetObjects("rbxassetid://77298741007658")[1]
                
                object.Parent = workspace
                wisp.Parent = object
                explosion.Parent = workspace
                PlaySound(5599556783)
                PlaySound(5862482798)
                PlaySound(9066732918)
                
                object.Position = targetCharacter.UpperTorso.Position 
                    + Vector3.new(0, math.random(20, 45), 0) 
                    + Vector3.new(math.random(-45, 45), math.random(5, 10), math.random(-45, 45))
                
                local tween1 = TweenService:Create(object, TweenInfo.new(0.25), {
                    Transparency = 0.55;
                    CFrame = CFrame.lookAt(object.Position, targetCharacter.UpperTorso.Position) * CFrame.Angles(-math.pi / 2, 0, 0);
                })
                
                local tween2 = TweenService:Create(object, TweenInfo.new(0.2, Enum.EasingStyle.Bounce, Enum.EasingDirection.In), {
                    Position = targetCharacter.UpperTorso.Position;
                })
                task.wait(0.35)
                tween1:Play()
                tween1.Completed:Connect(function()
                    tween2:Play()
                end)
                wisp.Rate = 100
                PlaySound(7093763783)
                PlaySound(4958429672)
                task.wait(.4)
                for _, part in pairs(targetCharacter:GetDescendants()) do 
                    if part:IsA("BasePart") or part:IsA("MeshPart") then 
                        TweenService:Create(part,TweenInfo.new(0.5),{
                            Transparency = 1
                        }):Play()
                    end
                end
                explosion.Position = targetCharacter.UpperTorso.Position
                explosion.Anchored = true
                task.wait(2)
                wisp.Rate = 0
                TweenService:Create(object, TweenInfo.new(1), { Transparency = 1 }):Play()
                TweenService:Create(explosion, TweenInfo.new(0.2), { Transparency = 1 }):Play()
                task.wait(1)
                game:GetService("Debris"):AddItem(object, 0)
                game:GetService("Debris"):AddItem(explosion, 0)
            end)
        end
        task.wait(1)
    end,

    ["Atomic"] = function(targetCharacter)
        local colorCorrection = Instance.new("ColorCorrectionEffect")
        colorCorrection.Brightness = 0.1
        colorCorrection.Contrast = 0.05
        colorCorrection.Saturation = 0.25
        colorCorrection.TintColor = Color3.fromRGB(232, 175, 255)
        colorCorrection.Parent = game.Lighting
        local debree = game:GetObjects("rbxassetid://96802982398078")[1]
        local beam = game:GetObjects("rbxassetid://71965261425891")[1]
        local bomb = game:GetObjects("rbxassetid://97354991062515")[1]
        local sfx = game:GetObjects("rbxassetid://107769536579891")[1]
        local hrp = targetCharacter:FindFirstChild("UpperTorso")
        debree.Parent = hrp
        debree.Enabled = true
        
        local sound1 = PlaySound(17760390080,2)
        task.delay(4.8,function()
            for i = 1, 3 do
                task.delay((i - 1) * 0.1, function()
                    local sword = Instance.new("MeshPart")
                    sword.Parent = workspace
                    sword.MeshId = "rbxassetid://12221720"
                    sword.CanCollide = false
                    sword.Anchored = true
                    sword.Color = Color3.fromRGB(161, 106, 255)
                    sword.Material = Enum.Material.Neon
                    sword.Size = Vector3.new(15, 19, 15)
            
                    for _, v in sfx:GetChildren() do
                        v.Parent = sword
                    end
            
                    sword.Position = hrp.Position + Vector3.new(0, math.random(20, 45), 0) + Vector3.new(math.random(-45, 45), math.random(5, 10), math.random(-45, 45))
            
                    local tween1 = TweenService:Create(sword, TweenInfo.new(0.25), { Transparency = 0.55; CFrame = CFrame.lookAt(sword.Position, hrp.Position) * CFrame.Angles(-math.pi / 1, 0, 0) })
                    local tween2 = TweenService:Create(sword, TweenInfo.new(0.25, Enum.EasingStyle.Bounce, Enum.EasingDirection.In), { Position = hrp.Position })
                    PlaySound(139771911328593,3)
                    tween1:Play()
                    tween1.Completed:Connect(function()
                        tween2:Play()
                    end)
            
                    tween2.Completed:Connect(function()
                        ShakeModule.StartShake(3, 0.5)
            
                        local att = Instance.new("Attachment", workspace.Terrain)
                        att.Position = hrp.Position
                        for _, v in beam:GetChildren() do
                            v.Parent = att
                            v.Rate = 100
                            v.Enabled = true
                        end
            
                        TweenService:Create(sword, TweenInfo.new(1), { Transparency = 1 }):Play()
                        game:GetService("Debris"):AddItem(sword, 1.2)
                        game:GetService("Debris"):AddItem(att, 1.2)
                    end)
                end)
            end
        end)
        
        sound1.Ended:Connect(function()
            local sound2 = PlaySound(113634674254896,2,.8)
            task.delay(.8, function()
                local att1 = Instance.new("Attachment", workspace.Terrain)
                att1.Position = hrp.Position
                for i, v in bomb:GetChildren() do
                    v.Parent = att1
                    v.Enabled = true
                end
        
                local beams = {}
                local widths = {}
        
                local function createBeam(name, textureId, brightness, color, textureLength, textureSpeed, width, zOffset, transparency)
                    local beam = Instance.new("Beam", workspace)
                    beam.Name = name
                    beam.Brightness = brightness
                    beam.Color = ColorSequence.new(color)
                    beam.LightEmission = 1
                    beam.LightInfluence = 0
                    beam.Texture = "rbxassetid://"..textureId
                    beam.TextureLength = textureLength
                    beam.TextureMode = "Static"
                    beam.TextureSpeed = textureSpeed
                    beam.FaceCamera = true
                    beam.Transparency = NumberSequence.new(transparency or 0)
                    beam.ZOffset = zOffset or 0
                    beam.Segments = 10
                    beam.Width0 = 0
                    beam.Width1 = 0
                    
                    widths[beam.Name] = width
                    table.insert(beams, beam)
                    return beam
                end
        
                local attachment0 = Instance.new("Attachment", workspace.Terrain)
                local attachment1 = Instance.new("Attachment", workspace.Terrain)
        
                attachment0.Position = hrp.Position + Vector3.new(0, 1000, 0)
                attachment1.Position = hrp.Position + Vector3.new(0, -3, 0)
        
                beams = {createBeam("Beam1", "12409325293", 3, Color3.fromRGB(201, 134, 255), 500, -2.2, 157.5),createBeam("Beam2", "12776597979", 5, Color3.fromRGB(196, 128, 255), 250, -2.5, 105),createBeam("Beam3", "12778438147", 2, Color3.fromRGB(201, 134, 255), 250, -1.75, 136.5, -1),createBeam("Beam4", "12614307736", 1, Color3.fromRGB(187, 128, 255), 150, -5, 105),createBeam("Beam5", "12951731980", 3, Color3.fromRGB(201, 134, 255), 250, -1.75, 157.5, -1),createBeam("Beam6", "12946867565", 1, Color3.fromRGB(202, 149, 255), 500, -1, 147),createBeam("Beam7", "12778438147", 3, Color3.fromRGB(184, 61, 255), 500, -2.5, 147, -1, 0, 0.5)}
        
                for i, beam in beams do
                    beam.Attachment0 = attachment0
                    beam.Attachment1 = attachment1
                    TweenService:Create(beam, TweenInfo.new(1.25, Enum.EasingStyle.Exponential), { Width0 = widths[beam.Name] }):Play()
                    TweenService:Create(beam, TweenInfo.new(1.25, Enum.EasingStyle.Exponential), { Width1 = widths[beam.Name] }):Play()
                end
        
                ShakeModule.StartShake(5, 4.5)
        
                task.delay(3.45, function()
                    for i, beam in beams do
                        TweenService:Create(beam, TweenInfo.new(1.25, Enum.EasingStyle.Linear), { Width0 = 0 }):Play()
                        TweenService:Create(beam, TweenInfo.new(1.25, Enum.EasingStyle.Linear), { Width1 = 0 }):Play()
                    end
                    task.wait(.3)
                    game:GetService("Debris"):AddItem(att1, 1)
                    game:GetService("Debris"):AddItem(colorCorrection, 1)
                    game:GetService("Debris"):AddItem(debree, 1)
                    game:GetService("Debris"):AddItem(attachment0, 1)
                    game:GetService("Debris"):AddItem(attachment1, 1)
                end)
            end)
        end)
    end,

    ["Tonka"] = function(targetCharacter)
        local bell = Instance.new("MeshPart")
        bell.MeshId = "rbxassetid://12108246376"
        bell.Anchored = true
        bell.Color = Color3.fromRGB(226, 155, 64)
        bell.Material = Enum.Material.Metal
        bell.Reflectance = 1
        bell.CanCollide = false
        bell.Size = Vector3.new(1, 1, 1)
        bell.Position = targetCharacter.UpperTorso.Position + Vector3.new(0, 20, 0)
        bell.Parent = workspace
        
        local a = game:GetObjects("rbxassetid://71342911226717")[1]
        local at = Instance.new("Attachment", workspace.Terrain)
        
        local colorEffects = {}
        for i = 1, 8 do
            local colorcor = Instance.new("ColorCorrectionEffect")
            colorcor.Parent = game.Lighting
            colorcor.Brightness = -0.1
            colorcor.Contrast = 0.3
            colorcor.Saturation = 1
            colorcor.TintColor = Color3.fromRGB(31, 236, 0)
            colorcor.Enabled = false
            table.insert(colorEffects, colorcor)
        end
        
        local tweens = {}
        local tweenService = game:GetService("TweenService")
        for _, colorcor in pairs(colorEffects) do
            local tween = tweenService:Create(colorcor, TweenInfo.new(1), {
                TintColor = Color3.fromRGB(255, 255, 255),
                Brightness = 0,
                Contrast = 0,
                Saturation = 0
            })
            table.insert(tweens, tween)
        end
        
        local startPos = bell.Position
        local tick = 0
        
        connection = game:GetService("RunService").RenderStepped:Connect(function(dt)
            tick += dt
            local xOffset = math.sin(tick * 1) * 10
            local zOffset = math.cos(tick * 1) * 10
            local yOffset = math.sin(tick * 0.6) * 10 * 0.6
            local newBellPosition = startPos + Vector3.new(xOffset, yOffset, zOffset)
            bell.Position = newBellPosition + Vector3.new(0, 0, 15)
            at.Position = newBellPosition
            bell.Orientation = Vector3.new(math.sin(tick * 0.6) * 360, math.sin(tick * 0.6) * 360, math.cos(tick * 0.6) * 360)
        end)
        
        task.wait(1)
        PlaySound(5689199277, 10)
        
        for _, colorcor in pairs(colorEffects) do
            colorcor.Enabled = true
        end
        
        for i, v in a:GetChildren() do
            v.Parent = at
            v.Enabled = true
        end
        
        for _, tween in pairs(tweens) do
            tween:Play()
        end
        
        game:GetService("Debris"):AddItem(at, 0.6)
        for _, colorcor in pairs(colorEffects) do
            game:GetService("Debris"):AddItem(colorcor, 1)
        end
        
        task.wait(2)
        connection:Disconnect()
        bell:Destroy()
    end,

    ["KillerQueen"] = function(targetCharacter)
        local bomb = game:GetObjects("rbxassetid://116213585790465")[1]
        local attachment = Instance.new("Attachment",workspace.Terrain)
        attachment.Position = targetCharacter.UpperTorso.Position
        PlaySound(6995653196)
        task.delay(1,function()
            local ColorCorrectionEffect = Instance.new("ColorCorrectionEffect", game.Lighting)
            ColorCorrectionEffect.TintColor = Color3.fromRGB(255, 144, 17)
            TweenService:Create(ColorCorrectionEffect, TweenInfo.new(1), {
            	TintColor = Color3.fromRGB(255, 255, 255);
            }):Play()
            
            PlaySound(8699897760)
            ShakeModule.StartShake(4, 1.5)
            for i,v in bomb:GetChildren() do
                v.Parent = attachment
                v.Enabled = true
            end
            
            task.delay(1.5,function()
                game:GetService("Debris"):AddItem(attachment,1)
            end)
        end)
    end,

    ["Broly"] = function(targetCharacter)
        function createMeshPart(parent, meshId, size, position, color)
            local mesh = Instance.new("MeshPart", parent)
            mesh.MeshId = meshId
            mesh.CastShadow = false
            mesh.Anchored = true
            mesh.CanCollide = false
            mesh.Material = Enum.Material.Neon
            mesh.Color = color
            mesh.Size = size
            mesh.Position = position
            return mesh
        end

        local beams = {}
        local widths = {}
        local beamTextures = {"9673419417", "9673419417", "9673423870"}
        local beamColors = {Color3.fromRGB(3, 180, 0), Color3.fromRGB(0, 255, 17), Color3.fromRGB(102, 255, 0)}
        
        local function fuckbeam(name, textureId, brightness, emission, color, textureLength, textureSpeed, width, zOffset, transparency)
            local beam = Instance.new("Beam", workspace)
            beam.Name = name
            beam.Brightness = brightness
            beam.Color = ColorSequence.new(color)
            beam.LightEmission = emission
            beam.LightInfluence = 0
            beam.Texture = "rbxassetid://" .. textureId
            beam.TextureLength = textureLength
            beam.TextureSpeed = textureSpeed
            beam.FaceCamera = true
            beam.Transparency = NumberSequence.new(transparency or 0)
            beam.ZOffset = zOffset or 0
            beam.Segments = 10
            beam.Width0 = 0
            beam.Width1 = 0
            widths[beam.Name] = width
            table.insert(beams, beam)
            return beam
        end
        
        local aura = game:GetObjects("rbxassetid://106918931794615")[1]
        local twight = game:GetObjects("rbxassetid://120813132066008")[1]
        local attachment = Instance.new("Attachment", workspace.Terrain)
        attachment.Position = Vector3.new(0, -3, 0)
        attachment.Orientation = Vector3.new(0, 0, -90)
        
        for _, v in aura:GetChildren() do
            for i = 1, 3 do
                local clonedPart = v:Clone()
                clonedPart.Name = "brolyaura"
                clonedPart.Parent = game.Players.LocalPlayer.Character.HumanoidRootPart
            end
        end
        
        for i,v in twight:GetChildren() do
            v.Parent = attachment
        end
        
        local colorcor = Instance.new("ColorCorrectionEffect", game.Lighting)
        colorcor.Enabled = true
        
        task.delay(0.5, function()
            TweenService:Create(game.Lighting, TweenInfo.new(3.5, Enum.EasingStyle.Circular), {GeographicLatitude = 360, FogEnd = 1000000}):Play()
            TweenService:Create(colorcor, TweenInfo.new(3.5, Enum.EasingStyle.Circular), {Brightness = -0.1, Contrast = 1, TintColor = Color3.fromRGB(140, 255, 179), Saturation = -0.5}):Play()
        end)
        
        PlaySound(5821817470, 1)
        
        task.delay(3.75, function()
            local debris = game:GetObjects("rbxassetid://73077107567880")[1]
            local attachment0 = Instance.new("Attachment", workspace.Terrain)
            local attachment1 = Instance.new("Attachment", workspace.Terrain)
            debris.Parent = attachment1
            debris.Enabled = true
            attachment0.Position = targetCharacter.UpperTorso.Position + Vector3.new(0, 1000, 0)
            attachment1.Position = targetCharacter.UpperTorso.Position + Vector3.new(0, -3, 0)
        
            local meshIds = {"rbxassetid://6026279770", "rbxassetid://6026280296", "rbxassetid://6026280813"}
            local shocks = {}
        
            for _, meshId in ipairs(meshIds) do
                local shock = createMeshPart(attachment1, meshId, Vector3.new(0.001, 0.001, 0.001), attachment1.Position + Vector3.new(0, math.random(50, 150), 0), Color3.fromRGB(0, 255, 0))
                table.insert(shocks, shock)
            end
        
            local crack = createMeshPart(attachment1, "rbxassetid://4663598929", Vector3.new(300, 300, 0.1),targetCharacter.UpperTorso.Position - Vector3.new(39, 3, 0), Color3.fromRGB(0, 255, 0))
            crack.Orientation = Vector3.new(90, 0, 0)
        
            local wind = createMeshPart(attachment1, "rbxassetid://5922912225", Vector3.new(29, 1700, 29),targetCharacter.UpperTorso.Position, Color3.fromRGB(0, 255, 0))
            TweenService:Create(wind, TweenInfo.new(7.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Orientation = Vector3.new(0, 9000, 0)}):Play()
            ShakeModule.StartShake(5, 6.5)
            for i = 1, 9 do
                local textureIndex = ((i - 1) % #beamTextures) + 1
                fuckbeam("Beam", beamTextures[textureIndex], 12, 1, beamColors[textureIndex], 1, 8, 100, 4, 0.85)
            end
        
            for _, shock in ipairs(shocks) do
                TweenService:Create(shock, TweenInfo.new(3, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut), {Transparency = 1, Size = Vector3.new(500, 50, 500), Orientation = Vector3.new(0, math.random(-360, 420), 0)}):Play()
            end
        
            for i, beam in ipairs(beams) do
                beam.Attachment0 = attachment0
                beam.Attachment1 = attachment1
                TweenService:Create(beam, TweenInfo.new(4, Enum.EasingStyle.Exponential), {Width0 = widths[beam.Name]}):Play()
                TweenService:Create(beam, TweenInfo.new(4, Enum.EasingStyle.Exponential), {Width1 = widths[beam.Name]}):Play()
            end
        
            task.delay(6, function()
                TweenService:Create(wind, TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Transparency = 1.1, Size = Vector3.new(1, 1700, 1), Orientation = Vector3.new(0, 9000, 0)}):Play()
                for _, beam in ipairs(beams) do
                    TweenService:Create(beam, TweenInfo.new(1.25, Enum.EasingStyle.Linear), {Width0 = 0}):Play()
                    TweenService:Create(beam, TweenInfo.new(1.25, Enum.EasingStyle.Linear), {Width1 = 0}):Play()
                end
                TweenService:Create(crack, TweenInfo.new(6, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut), {Transparency = 1}):Play()
                task.wait(0.3)
                game:GetService("Debris"):AddItem(colorcor, 1)
                game:GetService("Debris"):AddItem(wind, 1)
                game:GetService("Debris"):AddItem(crack, 1)
                game:GetService("Debris"):AddItem(attachment, 1)
                game:GetService("Debris"):AddItem(attachment0, 1)
                game:GetService("Debris"):AddItem(attachment1, 1)
        
                for i, beam in ipairs(beams) do
                    game:GetService("Debris"):AddItem(beam, 1)
                end
            end)
        end)
        
        task.delay(30, function()
            for i, v in game.Players.LocalPlayer.Character:GetDescendants() do
                if v.Name == "brolyaura" then
                    game:GetService("Debris"):AddItem(v, 1)
                end
            end
        end)
    end,

    ["Skyfall"] = function(targetCharacter)
        local star = game:GetObjects("rbxassetid://95291661466012")[1]
        local starlightlayer = game:GetObjects("rbxassetid://108462640730227")[1]
        local skylayer = game:GetObjects("rbxassetid://109520947799951")[1]
        local skylayer1 = game:GetObjects("rbxassetid://97920781096269")[1]
        local skylayer2 = game:GetObjects("rbxassetid://125420784983292")[1]
        local starA = Instance.new("Attachment",workspace.Terrain)
        local starL = Instance.new("Attachment",workspace.Terrain)
        local layer1A = Instance.new("Attachment",workspace.Terrain)
        local layer2A = Instance.new("Attachment",workspace.Terrain)
        local layer3A = Instance.new("Attachment",workspace.Terrain)
        starA.Position = targetCharacter.UpperTorso.Position + Vector3.new(0,500,0)
        starA.Orientation = Vector3.new(-90, 0, 0)
        starL.Position = targetCharacter.UpperTorso.Position + Vector3.new(0,430,0)
        starL.Orientation = Vector3.new(-90, 0, 0)
        layer1A.Position = targetCharacter.UpperTorso.Position + Vector3.new(0,425,0)
        layer1A.Orientation = Vector3.new(-90, 0, 0)
        layer2A.Position = targetCharacter.UpperTorso.Position + Vector3.new(0,350,0)
        layer2A.Orientation = Vector3.new(-90, 0, 0)
        layer3A.Position = targetCharacter.UpperTorso.Position + Vector3.new(0,210,0)
        layer3A.Orientation = Vector3.new(-90, 0, 0)
        for i, v in star:GetChildren() do
            v.Enabled = true
            v.Orientation = "VelocityPerpendicular"
            v.Parent = starA
        end
        skylayer2.Enabled = true
        skylayer2.Orientation = "VelocityPerpendicular"
        skylayer2.Parent = layer3A
        starlightlayer.Enabled = true
        starlightlayer.Orientation = "VelocityPerpendicular"
        starlightlayer.Parent = starL
        skylayer.Enabled = true
        skylayer.Orientation = "VelocityPerpendicular"
        skylayer.Parent = layer1A
        
        for i, v in skylayer1:GetChildren() do
            v.Enabled = true
            v.Orientation = "VelocityPerpendicular"
            v.Parent = layer2A
        end
        
        task.wait(.5)
        PlaySound(18286597024, 2)

        task.delay(.5, function()
            for i = 1, 3 do
                ShakeModule.StartShake(3, 0.5)
                local beam1 = Instance.new("Beam", workspace)
                local beam2 = Instance.new("Beam", workspace)
                beam1.Brightness = 4
                beam1.Color = ColorSequence.new(Color3.fromRGB(186, 113, 189))
                beam1.LightEmission = 1
                beam1.Texture = "rbxassetid://12778369763"
                beam1.TextureSpeed = 2
                beam1.FaceCamera = true
                beam1.Segments = 20
                beam1.Width0 = 17
                beam1.Width1 = 17
                
                local dimension1 = game:GetObjects("rbxassetid://114528023811639")[1]
                local wind1 = game:GetObjects("rbxassetid://72047300857414")[1]
                local clonedim = dimension1:Clone()
                local clonewind = wind1:Clone()
                local attachment0 = Instance.new("Attachment", workspace.Terrain)
                local attachment1 = Instance.new("Attachment", targetCharacter.UpperTorso)
                local attachment2 = Instance.new("Attachment", workspace.Terrain)
                local attachment3 = Instance.new("Attachment", workspace.Terrain)
                local attachment0b = Instance.new("Attachment", workspace.Terrain)
                local attachment1b = Instance.new("Attachment", targetCharacter.UpperTorso)
        
                attachment0.Position = targetCharacter.UpperTorso.Position + Vector3.new(0, 500, 0)
                attachment1.Position = Vector3.new(0, -3, 0)
                attachment0b.Position = targetCharacter.UpperTorso.Position + Vector3.new(0, 500, 0)
                attachment1b.Position = Vector3.new(0, -3, 0)
                attachment2.Position = targetCharacter.UpperTorso.Position
                attachment3.Position = targetCharacter.UpperTorso.Position + Vector3.new(0,-3,0)
                attachment2.Orientation = Vector3.new(-90, 0, 0)
        
                beam1.Attachment0 = attachment0
                beam1.Attachment1 = attachment1
                beam2.Attachment0 = attachment0b
                beam2.Attachment1 = attachment1b
                beam2.Brightness = 4
                beam2.Color = ColorSequence.new(Color3.fromRGB(186, 113, 189))
                beam2.LightEmission = 1
                beam2.FaceCamera = true
                beam2.Texture = "rbxassetid://12778357208"
                beam2.TextureSpeed = 4
                beam2.Segments = 20
                beam2.Width0 = 17
                beam2.Width1 = 17
        
                for _, v in clonedim:GetChildren() do
                    v.Parent = attachment2
                    v.Enabled = true
                end
                for _, v in clonewind:GetChildren() do
                    v.Parent = attachment3
                    v.Enabled = true
                end
                
                TweenService:Create(beam1, TweenInfo.new(0.5), {Width0 = 0, Width1 = 0}):Play()
                TweenService:Create(beam2, TweenInfo.new(0.5), {Width0 = 0, Width1 = 0}):Play()
        
                if i == 3 then
                    local real = PlaySound(18286597024, 2, 1.1)
                    task.wait(1.3)
                    local fadeOut = TweenService:Create(real, TweenInfo.new(1), { Volume = 0 })
                    fadeOut:Play()
                    fadeOut.Completed:Connect(function()
                        StopSound(real)
                    end)
                end

                for _, part in pairs(targetCharacter:GetDescendants()) do 
                    if part:IsA("BasePart") or part:IsA("MeshPart") then 
                        TweenService:Create(part,TweenInfo.new(0.5),{
                            Transparency = 1
                        }):Play()
                    end
                end

                task.delay(1.7, function()
                    game:GetService("Debris"):AddItem(beam1, 0)
                    game:GetService("Debris"):AddItem(beam2, 0)
                    game:GetService("Debris"):AddItem(attachment2, 0)
                    game:GetService("Debris"):AddItem(attachment3, 0)
                    game:GetService("Debris"):AddItem(colorCorrection, 2.5)
                    game:GetService("Debris"):AddItem(starA,1)
                    game:GetService("Debris"):AddItem(starL,1)
                    game:GetService("Debris"):AddItem(layer1A,1)
                    game:GetService("Debris"):AddItem(layer2A,1)
                    game:GetService("Debris"):AddItem(layer3A,1)
                end)
                task.wait(0.6)
            end
        end)
    end,

    ["BlackHole"] = function(targetCharacter)
        BlackHole.Parent = workspace
        BlackHole.Position = targetCharacter:FindFirstChild("UpperTorso").Position + Vector3.new(0,1,0)
        BlackHole.open:Play()
        ShakeModule.StartShake(7, 0.5)

        
        for _, emitter in pairs(BlackHole:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") or emitter:IsA("Beam") then 
                emitter.Enabled = true  
            end 
        end 

        task.wait(1.5)
        task.spawn(function()
            BlackHole.wind:Play()
            BlackHole.bring:Play()

            for _, part in pairs(targetCharacter:GetChildren()) do 
                if not part:IsA("BasePart") then continue end 
                TweenService:Create(part,TweenInfo.new(1.4),{
                    Position = BlackHole.Event.WorldPosition
                }):Play()
            end 
            task.wait(1.5)
            targetCharacter:Destroy()
        end)

        task.wait(3)
        TweenService:Create(BlackHole.wind,TweenInfo.new(0.5),{
            Volume = 0
        }):Play()

        for _, emitter in pairs(BlackHole:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") or emitter:IsA("Beam") then 
                emitter.Enabled = false 
            end 
        end 

        task.wait(2)
        BlackHole.Parent = ReplicatedStorage
    end,

    ["Spirit"] = function(targetCharacter)
        Spirit.Stomp:Play()
        Aura.Transparency = 0
        Aura.Position = targetCharacter:FindFirstChild("UpperTorso").Position 

        ShakeModule.StartShake(7, 0.5)

        TweenService:Create(Aura,TweenInfo.new(2),{
            Position = targetCharacter:FindFirstChild("UpperTorso").Position + Vector3.new(0,50,0),
            Size = Vector3.new(120, 90, 120),
            Transparency = 1
        }):Play()
        local Sound = Instance.new("Sound",workspace)
        Sound.SoundId = "rbxassetid://6290067239"
        Sound.PlayOnRemove = true 
        Sound:Destroy()

        for _, asset in pairs(targetCharacter:GetDescendants()) do 
            if not asset:IsA("BasePart") or not asset:IsA("Decal") then continue end 

            TweenService:Create(asset,TweenInfo.new(0.5),{
                Transparency = 1
            }):Play()
        end 
    end,

    ["Charm"] = function(targetCharacter)
        local Stomp = Charm.FX.Stomp 
        local Sound = Charm.SFX:FindFirstChild("Explosive Hit 5")
        local TargetPos = targetCharacter:FindFirstChild("UpperTorso").Position

        local Emitters = {}

        local PartOne = Stomp.PartOne:Clone()
        local Part    = Stomp.Part:Clone()

        Charm.Parent = workspace

        PartOne.Position = TargetPos
        PartOne.Anchored = true 
        PartOne.Parent = workspace

        Part.Position = TargetPos
        Part.Anchored = true 
        Part.Parent = workspace

        for _, emitter in pairs(PartOne:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") then 
                table.insert(Emitters,emitter)
            end 
        end 

        for _, emitter in pairs(Part:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") then 
                table.insert(Emitters,emitter)
            end 
        end

        for _, emitter in pairs(Emitters) do 
            emitter.Enabled = true
        end 

        local old_color = LightingService.FogColor
        Sound:Play()
        for _, asset in pairs(targetCharacter:GetDescendants()) do 
            if not asset:IsA("BasePart") or not asset:IsA("Decal") then continue end 

            TweenService:Create(asset,TweenInfo.new(0.5),{
                Transparency = 1
            }):Play()
        end 
        TweenService:Create(LightingService,TweenInfo.new(0.5),{
            FogColor = Color3.fromRGB(255, 42, 191)
        }):Play()

        task.wait(0.8)
        Part:Destroy()
        PartOne:Destroy()

        TweenService:Create(LightingService,TweenInfo.new(0.5),{
            FogColor = old_color
        }):Play()
        Charm.Parent = ReplicatedStorage
    end,

    ["Thanos"] = function(targetCharacter)
        local Sound = Instance.new("Sound",workspace)
        Sound.SoundId = "rbxassetid://3050376525"
        Sound:Play()

        for _, part in pairs(targetCharacter:GetDescendants()) do 
            if part:IsA("BasePart") or part:IsA("MeshPart") then 
                local Thanos_Effect = Thanos:Clone() 
                Thanos_Effect.Parent = part 
                Thanos_Effect.Color =  ColorSequence.new({ ColorSequenceKeypoint.new(0, part.Color), ColorSequenceKeypoint.new(0.6, Color3.fromRGB(65, 33, 18)), ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 0)) })

                TweenService:Create(part,TweenInfo.new(0.5),{
                    Transparency = 1
                }):Play()

                task.delay(0.5,function()
                    Thanos_Effect.Enabled = false 
                end)
                task.wait(.1)
            end 
        end
        repeat task.wait() until Sound.TimeLength == 0
        Sound:Destroy()
    end,

    ["Afterslash"] = function(targetCharacter)
        local AfterSlashEfx = AfterSlash:Clone()
        local UpperTorso = targetCharacter:FindFirstChild("UpperTorso")

        local Part = AfterSlashEfx.Part
        Part.Position = UpperTorso.Position
        Part.Parent = UpperTorso
        Part.Size = Vector3.new(5,7,5)

        for _ = 1, 25 do 
            task.wait(math.random(1,9) / 100)
            local Sound = Instance.new("Sound",Part)
            Sound.SoundId = "rbxassetid://5989945551"
            local DistortionSoundEffect = Instance.new("DistortionSoundEffect",Sound)
            Sound.PlayOnRemove = true 
            Sound:Destroy()
            Part.Position = UpperTorso.Position
        end

        UpperTorso.Position = (CFrame.new(UpperTorso.Position) * CFrame.new(0, -5, 0)).Position
        for _, emitter in pairs(Part:GetDescendants()) do
            if emitter:IsA("ParticleEmitter") then
                emitter.Enabled = false
            end
        end

        task.wait(0.25)
        local Attachment = Instance.new("Attachment",workspace.Terrain)
        Attachment.WorldPosition = UpperTorso.Position

        local Sound = Instance.new("Sound",Part)
        Sound.SoundId = "rbxassetid://4471648128" 
        Sound.PlayOnRemove = true
        Sound:Destroy()

        for _, emitter in pairs(AfterSlashEfx.Burst:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") then 
                emitter.Parent = Attachment
                emitter:Emit(emitter:GetAttribute("EmitCount"))
            end 
        end 

        for _, part in pairs(targetCharacter:GetDescendants()) do
            if part:IsA("BasePart") then
                TweenService:Create(part, TweenInfo.new(0.5, Enum.EasingStyle.Sine), {
                    Transparency = 1
                }):Play()
            end
        end 
    end,

    ["Rune"] = function(targetCharacter)
        local Rune_fx = Rune:Clone()
            
        local UpperTorso = targetCharacter:FindFirstChild("UpperTorso")
        local OutPosition = UpperTorso.Position + Vector3.new(0,120,0)

        local Effects = {}

        Rune_fx.Parent = workspace

        for i = 5,1, -1 do 
            if i == 1 then 
                local Part = Rune_fx["1"]
                Part.Position = OutPosition
                Part.Sound:Play()

                TweenService:Create(Part,TweenInfo.new(1.25, Enum.EasingStyle.Quint, Enum.EasingDirection.InOut, 0), {
                    Size = Vector3.new(60,0,60),
                    Position = OutPosition
                }):Play()

                table.insert(Effects,Part)

                for _, decal in pairs(Part:GetChildren()) do 
                    if decal:IsA("Decal") then 
                        TweenService:Create(decal,TweenInfo.new(0.75, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {
                            Transparency = 0
                        }):Play()
                    end 
                end 
            elseif i <= 4 then 
                local Part = Rune_fx[tostring(i)]
                Part.Position = OutPosition
                Part.Sound:Play()

                TweenService:Create(Part,TweenInfo.new(1.25, Enum.EasingStyle.Quint, Enum.EasingDirection.InOut, 0), {
                    Size = Vector3.new(20 * i,0,20 * i),
                    Position = OutPosition
                }):Play()

                table.insert(Effects,Part)

                for _, decal in pairs(Part:GetChildren()) do 
                    if decal:IsA("Decal") then
                        TweenService:Create(decal,TweenInfo.new(0.75, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {
                            Transparency = 0
                        }):Play()
                    end 
                end 
            elseif i == 5 then 
                local Part = Rune_fx.Part3
                Part.Position = OutPosition
                table.insert(Effects,Part)

                TweenService:Create(Part,TweenInfo.new(1.25, Enum.EasingStyle.Quint, Enum.EasingDirection.InOut, 0), {
                    Size = Vector3.new(89,0,89),
                    Position = OutPosition
                }):Play()

                for _, decal in pairs(Part:GetChildren()) do 
                    if decal:IsA("Decal") then 
                        TweenService:Create(decal,TweenInfo.new(0.75, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {
                            Transparency = 0
                        }):Play()
                    end 
                end
            end 
            OutPosition = OutPosition - Vector3.new(0,15,0)
            task.wait(i / 6)
        end
        
        task.wait(1)
        for index, effect in pairs(Effects) do 
            TweenService:Create(effect,TweenInfo.new(0.7 + index / 4, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, 0),{
                Size = effect.Size + Vector3.new(effect.Size.Z / 2 + 80 + index * 3,0, effect.Size.Z / 2 + 80 + index * 3),
                Position = effect.Position + Vector3.new(0,effect.Position.Y + 20 + index * 5,0)
            })

            for i, decal in pairs(effect:GetChildren()) do 
                if decal:IsA("Decal") then 
                    TweenService:Create(decal,TweenInfo.new(0.7 + i / 4, Enum.EasingStyle.Quint, Enum.EasingDirection.InOut), {
                        Transparency = 1
                    }):Play()
                end 
            end
        end 

        local CYLD = Rune_fx.CYLD
        CYLD.Anchored = true 
        CYLD.CanCollide = false 
        CYLD.Color = Color3.fromRGB(20,122,255)
        CYLD.Transparency = 0.2
        CYLD.Material = Enum.Material.Neon
        CYLD.Shape = Enum.PartType.Cylinder 

        local Sound = Instance.new("Sound",workspace)
        Sound.SoundId = "rbxassetid://6808975002"
        Sound:Play()

        local UpperTorsoPos = UpperTorso.Position
        local rayOrigin = Vector3.new(UpperTorsoPos.X,UpperTorsoPos.Y + 400,UpperTorsoPos.Z)
        local rayDirection = Vector3.new(UpperTorsoPos.X,UpperTorsoPos.Y - 20 ,UpperTorsoPos.Z)

        local hitPart,hitPosition = workspace:FindPartOnRayWithIgnoreList(
            Ray.new(rayOrigin, ( rayDirection - rayOrigin ).unit * 500),
            { workspace.Ignored, workspace.Players }
        )
        local Magnitude = ( rayOrigin - hitPosition ).Magnitude
        CYLD.Size = Vector3.new(Magnitude,0.5,0.5)
        CYLD.CFrame = CFrame.new(rayOrigin,rayDirection) * CFrame.new(0,0, -Magnitude / 2) * CFrame.Angles(0, 1.5707963267948966, 0)

        coroutine.wrap(function()
            task.spawn(function()
                while task.wait() do 
                    for _, beam in pairs(CYLD:GetChildren()) do
                        if beam:IsA("Beam") then
                            CYLD.Attachment.WorldPosition = CYLD.Position + Vector3.new(0, CYLD.Size.X, 0);
                            CYLD.ATT.WorldPosition = CYLD.Position + Vector3.new(0, -CYLD.Size.X, 0);
                            beam.Width0 = CYLD.Size.Z / 1.5;
                            beam.Width1 = CYLD.Size.Z / 1.5;
                        end
                    end
                    if CYLD.Parent == nil then 
                        break 
                    end 
                end
            end) 

            local Tween = TweenService:Create(CYLD,TweenInfo.new(0.5),{
                Transparency = 0.8,
                Size = Vector3.new(Magnitude,30,30)
            })
            Tween:Play()
            Tween.Completed:Wait()

            TweenService:Create(CYLD,TweenInfo.new(2), {
                Transparency = 1,
                Size = Vector3.new(Magnitude,0,0)
            }):Play()
        end)()
        local Shock_fx = Shock:Clone()
        Shock_fx.Parent = targetCharacter
        Shock_fx.Position = UpperTorsoPos
        TweenService:Create(Shock_fx,TweenInfo.new(2.5, Enum.EasingStyle.Linear, Enum.EasingDirection.In),{
            Transparency = 1,
            Size = Shock_fx.Size + Vector3.new(100,20,100),
            Orientation = Vector3.new(0,200,0),
            Position = UpperTorsoPos
        }):Play()

        local Wind_fx = Wind:Clone()
        Wind_fx.Parent = targetCharacter
        Wind_fx.Position = UpperTorsoPos

        TweenService:Create(Wind_fx,TweenInfo.new(3, Enum.EasingStyle.Linear, Enum.EasingDirection.In),{
            Transparency = 1,
            Size = Wind_fx.Size + Vector3.new(100,5,100),
            Orientation = Vector3.new(0,-200,0),
            Position = UpperTorsoPos
        }):Play()

        local Root_fx = Root:Clone()
        Root_fx.Parent = targetCharacter
        Root_fx.Position = UpperTorsoPos

        TweenService:Create(Root_fx,TweenInfo.new(3, Enum.EasingStyle.Linear, Enum.EasingDirection.In),{
            Transparency = 1,
            Size = Root_fx.Size + Vector3.new(100,100,100),
            Orientation = Vector3.new(0,-200,0),
            Position = UpperTorsoPos
        }):Play()
        ShakeModule.StartShake(7, 0.5)

        local Sound = Instance.new("Sound",targetCharacter)
        Sound.SoundId = "rbxassetid://6554844728"
        Sound:Play()

        local Sound = Instance.new("Sound",targetCharacter)
        Sound.SoundId = "rbxassetid://6230611917"
        Sound:Play()

        task.wait(2.5)
        Shock_fx:Destroy()
        task.wait(0.5)
        Rune_fx:Destroy()
        Root_fx:Destroy()
        Wind_fx:Destroy()      
    end,

    ["HollowPurple"] = function(targetCharacter)
        local HollowPurple_fx = HollowPurple:Clone()
        HollowPurple_fx.Parent = workspace

        local HollowRed = HollowPurple_fx.HollowRed
        HollowRed.spawn:Play()

        for _, emitter in pairs(HollowRed:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") then 
                emitter:Emit(emitter:GetAttribute("EmitCount"))
            end 
        end 

        local HumanoidRootPart = LocalPlayer.Character.HumanoidRootPart
        local UpperTorso = targetCharacter.UpperTorso

        DhLibrary:AddConnection("Hollow_Red_Tp",RunService.Heartbeat:Connect(function()
            HollowRed.CFrame = CFrame.lookAt(HumanoidRootPart.Position,UpperTorso.Position) * CFrame.new(-8,2,-8)
        end))

        task.wait(.65)

        local HollowBlue = HollowPurple_fx.HollowBlue
        HollowBlue.spawn:Play()

        DhLibrary:AddConnection("Hollow_Blue_Tp",RunService.Heartbeat:Connect(function()
            HollowBlue.CFrame = CFrame.lookAt(HumanoidRootPart.Position,UpperTorso.Position) * CFrame.new(8,2,-8)
        end))

        for _, emitter in pairs(HollowBlue:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") then 
                emitter:Emit(emitter:GetAttribute("EmitCount"))
            end 
        end

        task.wait(.65)
        DhLibrary:RemoveConnection("Hollow_Blue_Tp")
        DhLibrary:RemoveConnection("Hollow_Red_Tp")

        local time = os.clock() + 1

        DhLibrary:AddConnection("Hollow_Red_Tp",RunService.Heartbeat:Connect(function()
            HollowRed.CFrame = (CFrame.lookAt(HumanoidRootPart.Position,UpperTorso.Position) * CFrame.new(-8,2,-8)):Lerp(
                CFrame.lookAt(
                    HumanoidRootPart.Position, 
                    UpperTorso.Position
                ) * CFrame.new(0, 2, -8), 1 - math.clamp(time - os.clock(), 0.001, 1)
            )
        end))

        DhLibrary:AddConnection("Hollow_Blue_Tp",RunService.Heartbeat:Connect(function()
            HollowBlue.CFrame = (CFrame.lookAt(HumanoidRootPart.Position, UpperTorso.Position) * CFrame.new(8, 2, -8)):Lerp(
                CFrame.lookAt(
                    HumanoidRootPart.Position, 
                    UpperTorso.Position
                ) * CFrame.new(0, 2, -8), 1 - math.clamp(time - os.clock(), 0.001, 1)
            )
        end))

        local HollowPurple_p = HollowPurple_fx.Purple 
        HollowPurple_p.Anchored = true 
        local SpawnSound = HollowPurple_p.Spawn
        SpawnSound.Parent = HumanoidRootPart
        SpawnSound:Play()
        task.wait(.7)

        for _, emitter in pairs(HollowBlue:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") then 
                emitter.Enabled = false
            end 
        end

        for _, emitter in pairs(HollowRed:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") then 
                emitter.Enabled = false
            end 
        end 
        DhLibrary:AddConnection("HollowPurpleTP",RunService.Heartbeat:Connect(function(time)
            HollowPurple_p.CFrame = CFrame.lookAt(HumanoidRootPart.Position,UpperTorso.Position) * CFrame.new(0,2,-8)
        end))
        task.wait(.9)
        DhLibrary:RemoveConnection("HollowPurpleTP")
        DhLibrary:RemoveConnection("Hollow_Blue_Tp")
        DhLibrary:RemoveConnection("Hollow_Red_Tp")
        -- Launch Sound
        local Sound = HollowPurple_p:FindFirstChildOfClass("Sound")
        Sound.Parent = HumanoidRootPart
        Sound:Play()
        ShakeModule.StartShake(7, 0.5)

        HollowPurple_p.Smoke.Dust.Enabled = true 
        HollowPurple_p.Smoke.Rocks.Enabled = true 

        for _, emitter in pairs(HollowPurple_p.Wind:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") then 
                emitter.Enabled = true 
            end 
        end 

        for _, emitter in pairs(HollowPurple_p.Launch:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") then 
                emitter:Emit(emitter:GetAttribute("EmitCount"))
            end 
        end

        local LookVector = CFrame.lookAt(HumanoidRootPart.Position,UpperTorso.Position).LookVector.Unit * 140 * Vector3.new(1,0,1)
        DhLibrary:AddConnection("HollowPurpleTP",RunService.Heartbeat:Connect(function(time)
            HollowPurple_p.Position = HollowPurple_p.Position + LookVector * time 
        end))

        DhLibrary:AddConnection("TeleportPlayer",RunService.Heartbeat:Connect(function()
            for _, part in pairs(targetCharacter:GetChildren()) do 
                if part:IsA("BasePart") then 
                    part.CFrame = CFrame.new(HollowPurple_p.Position)
                end 
            end 
        end))


        local Blur = HollowPurple_fx.Blur 
        local ColorCorrection = HollowPurple_fx.ColorCorrection

        Blur.Parent = Camera 
        ColorCorrection.Parent = Camera 

        TweenService:Create(Blur,TweenInfo.new(0.15),{
            Size = 24
        }):Play()

        TweenService:Create(ColorCorrection,TweenInfo.new(0.15),{
            Contrast = 6
        }):Play()

        task.delay(0.05,function()  
            TweenService:Create(Blur,TweenInfo.new(0.15),{
                Size = 0
            }):Play()

            TweenService:Create(ColorCorrection,TweenInfo.new(0.15),{
                Contrast = 0
            }):Play()
            task.wait(0.5)
        end)

        task.wait(3)

        for _, emitter in pairs(HollowPurple_p:GetDescendants()) do 
            if emitter:IsA("ParticleEmitter") then 
                emitter.Enabled = false  
            end 
        end 

        task.wait(3)
        HollowPurple_fx:Destroy()
        DhLibrary:RemoveConnection("HollowPurpleTP")
        DhLibrary:RemoveConnection("TeleportPlayer")        
    end,
}
local function effect(targetCharacter)
    local currentTime = tick()
    if currentTime - StompModule.lastStompTime < StompModule.StompCooldown then 
        return 
    end
    StompModule.lastStompTime = currentTime
    if Effects[StompModule.StompEffect] then 
        Effects[StompModule.StompEffect](targetCharacter,LocalPlayer.Character)
    end 
end 

function StompModule:AddStompEffect(options)
    local Name: String = options.Name
    local Callack:  (...any) -> ...any = options.Callback
    
    Effects[Name] = Callback
    table.insert(StompModule.CurrentEffects,Name)
end 

function StompModule:Toggle(state)
    if not state then 
        DhLibrary:RemoveConnection("StompEffect")
        return 
    end 

    DhLibrary:AddConnection("StompEffect", RunService.Heartbeat:Connect(function()
        local playerCharacter = LocalPlayer.Character
        if not playerCharacter or not playerCharacter:FindFirstChild("LowerTorso") or not playerCharacter:FindFirstChild("UpperTorso") then
            return
        end
        
        local rayOrigin = playerCharacter.LowerTorso.Position
        local rayDirection = Vector3.new(0, -playerCharacter.UpperTorso.Size.y * 4.5, 0)
        
        local whitelist = {}
        for _, player in ipairs(Services.Players:GetPlayers()) do
            if player ~= Services.LocalPlayer and player.Character and not  whitelist[player.Character] then
                table.insert(whitelist, player.Character)
            end
        end
        
        if #whitelist > 0 then
            local hitPart, hitPosition, hitNormal = workspace:FindPartOnRayWithWhitelist(
                Ray.new(rayOrigin, rayDirection),
                whitelist,
                false,
                true
            )
            
            if hitPart then
                local stompedCharacter = hitPart:FindFirstAncestorOfClass("Model")
                if stompedCharacter and stompedCharacter:FindFirstChild("HumanoidRootPart") and stompedCharacter:FindFirstChild("BodyEffects") and stompedCharacter.BodyEffects.SDeath.Value and DhLibrary:IsAnimPlaying(playerCharacter,2816431506) then
                    effect(stompedCharacter)
                end
            end
        end
    end))
end 

local HttpService = game:GetService("HttpService")
local fileName = "stompEffect.json"
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local SoundService = game:GetService("SoundService")

local function saveEffect(effect)
    local data = { selectedEffect = effect }
    writefile(fileName, HttpService:JSONEncode(data))
end

local function loadEffect()
    if isfile(fileName) then
        local data = HttpService:JSONDecode(readfile(fileName))
        return data.selectedEffect
    else
        local defaultEffect = "Atomic"
        saveEffect(defaultEffect)
        return defaultEffect
    end
end

for name, func in pairs(Effects) do 
    table.insert(StompModule.CurrentEffects, name)
end

local savedStompEffect = loadEffect()
StompModule.StompEffect = savedStompEffect

StompModule:Toggle(true)

local function showPopup(message, duration)
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = player:FindFirstChild("PlayerGui")
    
    local textLabel = Instance.new("TextLabel")
    textLabel.Parent = screenGui
    textLabel.Size = UDim2.new(0, 300, 0, 75)
    textLabel.Position = UDim2.new(0.5, -150, 0, 10)
    textLabel.BackgroundTransparency = 1
    textLabel.TextColor3 = Color3.new(1, 1, 1)
    textLabel.Text = message
    textLabel.Font = Enum.Font.SourceSansBold
    textLabel.TextSize = 30
    textLabel.BackgroundTransparency = 1
    textLabel.BorderSizePixel = 0
    
    local tweenService = game:GetService("TweenService")
    local fadeIn = tweenService:Create(textLabel, TweenInfo.new(1), {TextTransparency = 0})
    fadeIn:Play()
    
    local sound = Instance.new("Sound")
    sound.Parent = SoundService
    sound.SoundId = "rbxassetid://97881181065416"
    sound.Volume = 1
    sound:Play()
    
    task.wait(duration)
    
    local fadeOut = tweenService:Create(textLabel, TweenInfo.new(1), {TextTransparency = 1})
    fadeOut:Play()
    fadeOut.Completed:Wait()
    
    screenGui:Destroy()
end

showPopup("Stomp effect Loaded! (" .. savedStompEffect .. ")", 5)

return StompModule, DhLibrary
