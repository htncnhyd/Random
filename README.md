# Random
Hdyjtgywu
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local hrp = character:WaitForChild("HumanoidRootPart")

local function onBodyVelocityAdded(bodyVelocity)
    if bodyVelocity:IsA("BodyVelocity") then
        bodyVelocity.Velocity = Vector3.new(bodyVelocity.Velocity.X, 0, bodyVelocity.Velocity.Z)
    end
end
character.DescendantAdded:Connect(onBodyVelocityAdded)

local moveSet = {
    punch1 = { animationId = "rbxassetid://12273188754" },
    punch2 = { animationId = "rbxassetid://12272894215" },
    punch3 = { animationId = "rbxassetid://10469639222" },
    punch4 = { animationId = "rbxassetid://10469643643" },
    move1 = { animationId = "rbxassetid://10468665991" },
    move2 = { animationId = "rbxassetid://10466974800" },
    move3 = { animationId = "rbxassetid://10471336737" },
    move4 = { animationId = "rbxassetid://12510170988" },
    dash = { animationId = "rbxassetid://10479335397" },
    ult = { animationId = "rbxassetid://12447707844" },
    wallcombo = { animationId = "rbxassetid://15955393872" },
    move5 = { animationId = "rbxassetid://12983333733" }
}

local replacementMoveset = {
    punch1 = { animationId = "rbxassetid://13532600125", startingTime = 0, endingTime = 1.5 },
    punch2 = { animationId = "rbxassetid://13295919399", startingTime = 0, endingTime = 1.8 },
    punch3 = { animationId = "rbxassetid://17889458563", startingTime = 0, endingTime = 2 },
    punch4 = { animationId = "rbxassetid://13378708199", startingTime = 0, endingTime = 2.2 },
    move1 = { animationId = "rbxassetid://140164642047188", startingTime = 6.8, endingTime = 7.97, speed = 1 },
    move2 = { animationId = "rbxassetid://13560306510", startingTime = 0.56, endingTime = 8.37, speed = 3 },
    move3 = { animationId = "rbxassetid://17838619895", startingTime = 0.3, endingTime = 2.2 },
    move4 = { animationId = "rbxassetid://13497875049", startingTime = 0, endingTime = 3.5, speed = 1.25 },
    dash = { animationId = "rbxassetid://17838006839", startingTime = 0, endingTime = 1 },
    ult = { animationId = "rbxassetid://106778226674700", startingTime = 0, endingTime = 3 },
    wallcombo = { animationId = "rbxassetid://76530443909428", startingTime = 0, endingTime = 3, speed = 1.5 },
    move5 = { animationId = "rbxassetid://18897118507", startingTime = 2, endingTime = 10, speed = 1 }
}

local function replaceMoveAnimation(humanoid)
    humanoid.AnimationPlayed:Connect(function(animation)
        for moveName, moveData in pairs(moveSet) do
            if animation.Animation.AnimationId == moveData.animationId then
                print("Original move detected: " .. moveName)

                -- Get the corresponding replacement animation
                local replacementAnimation = replacementMoveset[moveName]
                if not replacementAnimation then return end

                -- Stop all animations before playing the new one
                for _, track in pairs(humanoid:GetPlayingAnimationTracks()) do
                    track:Stop()
                end

                -- Create new animation
                local anim = Instance.new("Animation")
                anim.AnimationId = replacementAnimation.animationId
                local animTrack = humanoid:LoadAnimation(anim)
                
                animTrack:Play()
                animTrack.TimePosition = replacementAnimation.startingTime

                if replacementAnimation.speed then
                    animTrack:AdjustSpeed(replacementAnimation.speed)
                end

                local duration = replacementAnimation.endingTime - replacementAnimation.startingTime
                local adjustedDuration = duration
                if replacementAnimation.speed then
                    adjustedDuration = duration / replacementAnimation.speed
                end

                if adjustedDuration > 60 then
                    return
                end

				if replacementAnimation.animationId == "rbxassetid://18897118507" then
                  if character.CameraRig then
                    character.CameraRig:Destroy()
                  end

				  if character.NoRotate then
                    character.NoRotate:Destroy()
                  end

				  task.wait(5.6)

				  message = "One Inch Killer Punch | 一寸杀拳"
                  game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(message, "All")
                end

                wait(adjustedDuration)

                animTrack:Stop()

                break
            end
        end
    end)
end

if humanoid then
    replaceMoveAnimation(humanoid)
end

local textLabel = player.PlayerGui:FindFirstChild("ScreenGui") and player.PlayerGui.ScreenGui:FindFirstChild("MagicHealth") and player.PlayerGui.ScreenGui.MagicHealth:FindFirstChild("TextLabel")
local hotbarFrame = player.PlayerGui:FindFirstChild("Hotbar"):FindFirstChild("Backpack"):FindFirstChild("Hotbar")

local toolTable = {
    ["Normal Punch"] = "The Punchiest Punch",
    ["Consecutive Punches"] = "The Consecutivest Punches",
    ["Shove"] = "The Pushiest Push",
    ["Uppercut"] = "The Uppercutiest Uppercut"
}

while true do

    for i = 1, 9 do
        local baseButton = hotbarFrame:FindFirstChild(tostring(i)) and hotbarFrame[tostring(i)].Base
        if baseButton then
            local oldName = baseButton.ToolName.Text
            local newName = toolTable[oldName]
            if newName then
                baseButton.ToolName.Text = newName
            end
        end
    end

    if textLabel and textLabel.Text == "SERIOUS MODE" then
        local selectedName = "SAITAMA'S AWAKEN"
        textLabel.Text = ""
        for i = 1, #selectedName do
            textLabel.Text = string.sub(selectedName, 1, i)
            wait(0.1)
        end
    end

    game:GetService("RunService").Heartbeat:Wait()
end
