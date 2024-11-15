local replicated_first = game:GetService("ReplicatedFirst");
local players = game:GetService("Players");
local workspace = game:GetService("Workspace");

local framework = require(replicated_first.Framework);
local wrapper = getupvalue(getupvalue(framework.require, 1), 1);

local classes = wrapper.Classes;
local libraries = wrapper.Libraries;

local bullets = libraries.Bullets;

local camera = workspace.CurrentCamera;
local get_closest_player = function()
    local closest_distance = 100;
    local closest_player = nil;

    for _, player in players:GetChildren() do
        local is_player = player.ClassName == "Player" and (player ~= players.LocalPlayer);

        if (is_player == false) then
            continue;
        end
        
        local character = player.Character;

        if (character == nil) then
            continue;
        end

        if (character.Head == nil) then
            continue;
        end

        local position, on_screen = camera:WorldToViewportPoint(character.Head.Position);

        if (on_screen == false) then
            continue;
        end

        local center = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2);
        local distance = (center - Vector2.new(position.X, position.Y)).Magnitude;

        if (closest_distance > distance) then
            closest_player = character;
            closest_distance = distance;
        end
    end

    return closest_player;
end

old_fire = hookfunction(bullets.Fire, function(weapon_data, character_data, _, gun_data, origin, direction, ...)
    local closest_character = get_closest_player();
    local current_gun = gun_data.__item; -- unused for now, feel free to do something with it tho
    
    if (closest_character and current_gun) then
        direction = (closest_character.Head.Position - origin).Unit;
    end
    return old_fire(weapon_data, character_data, _, gun_data, origin, direction, ...);
end)
