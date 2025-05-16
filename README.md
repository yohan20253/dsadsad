local ReplicatedStorage = game:GetService("ReplicatedStorage")
local ToggleCar = ReplicatedStorage:WaitForChild("ToggleCar")
local carModel = ReplicatedStorage:WaitForChild("CarModel")

-- Tabela para armazenar os personagens originais
local originalCharacters = {}

ToggleCar.OnServerEvent:Connect(function(player, turnIntoCar)
	if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return end

	if turnIntoCar then
		-- Salva o personagem original (se ainda não salvo)
		if not originalCharacters[player] then
			originalCharacters[player] = player.Character:Clone()
		end

		-- Clona o carro e move para a posição do jogador
		local carClone = carModel:Clone()
		carClone.Name = player.Name
		carClone:SetPrimaryPartCFrame(player.Character.PrimaryPart.CFrame)

		-- Substitui o personagem
		player.Character:Destroy()
		carClone.Parent = workspace
		player.Character = carClone
	else
		-- Restaura o personagem original
		local original = originalCharacters[player]
		if original and original:FindFirstChild("HumanoidRootPart") then
			local pos = player.Character:GetPrimaryPartCFrame()
			local newChar = original:Clone()
			newChar:SetPrimaryPartCFrame(pos)
			player.Character:Destroy()
			newChar.Parent = workspace
			player.Character = newChar
		end
	end
end)
# dsadsad
