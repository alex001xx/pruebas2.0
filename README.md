-- EXTRAÍDO DEL KAISER HUB: AUTOSHOOT Y AIMBOT DE CÁMARA.
-- Edita SETTINGS para activar/desactivar las funciones.
-- No hay un Silent Aim independiente en el archivo original: AutoShoot apunta
-- temporalmente la cámara/personaje, activa el Tool y restaura su orientación.
-- No se añadieron hooks de Raycast ni de __namecall.
-- Las funciones auxiliares se conservaron de la reconstrucción del original.
-- InstantKillTarget es una dependencia opcional del hub original; su llamada
-- está dentro de pcall y este archivo no incluye su implementación.
local SETTINGS = {
    ScriptActive = true,
    AimbotEnabled = false,
    AutoShootEnabled = false,
    AimbotWallCheck = true,
    AimbotTeamCheck = true,
    AimbotPartIndex = 1, -- 1: cabeza; 2: torso superior/cuello; 3: torso inferior
    FOVRadius = 150,
    FOVIsMobile = false,
}
local AUTO_SHOOT_INTERVAL = 1.2
local SAFE_ZONES = {
    {Centro = Vector3.new(-320.5, 280.82, 16), Radio = 500},
    {Centro = Vector3.new(1564.14, -155.45, 40.04), Radio = 300},
}
local unpack_values = table.unpack or unpack
local function pack_values(...) return {n = select("#", ...), ...} end
local runtime_env = (getfenv and getfenv()) or _ENV or _G
local function cell(value) return {value = value} end
local function bind(callback, captures)
    return function(...) return callback(captures, {...}) end
end
local callbacks = {}

callbacks.callback_55 = function(captures, arguments)
    local B, I, X, branch_result, l
    local e7 = {}
    do
        local temp_1
        temp_1 = arguments[1]
        branch_result = (not temp_1)
        X = temp_1
    end
    if branch_result then
        return unpack_values({nil})
    end
    do
        local temp_4
        temp_4 = captures[1].value.AimbotPartIndex
        branch_result = (temp_4 == 1)
        B = temp_4
    end
    if (not branch_result) then
        branch_result = (B == 2)
        if (not branch_result) then
            branch_result = (B == 3)
            if (not branch_result) then
                return unpack_values({unpack_values({X.FindFirstChild(X, "HumanoidRootPart")})})
            end
            do
                local temp_4
                temp_4 = X.FindFirstChild(X, "LowerTorso")
                branch_result = temp_4
                I = temp_4
            end
            if (not branch_result) then
                do
                    local temp_4
                    temp_4 = X.FindFirstChild(X, "Torso")
                    branch_result = temp_4
                    l = temp_4
                end
                if (not branch_result) then
                    l = X.FindFirstChild(X, "HumanoidRootPart")
                end
                I = l
            end
            return unpack_values({I})
        end
        do
            local temp_4
            temp_4 = X.FindFirstChild(X, "UpperTorso")
            branch_result = temp_4
            I = temp_4
        end
        if (not branch_result) then
            do
                local temp_4
                temp_4 = X.FindFirstChild(X, "Torso")
                branch_result = temp_4
                l = temp_4
            end
            if (not branch_result) then
                l = X.FindFirstChild(X, "Head")
            end
            I = l
        end
        return unpack_values({I})
    end
    do
        local temp_4
        temp_4 = X.FindFirstChild(X, "Head")
        branch_result = temp_4
        I = temp_4
    end
    if (not branch_result) then
        I = X.FindFirstChild(X, "HumanoidRootPart")
    end
    return unpack_values({I})
end

callbacks.callback_56 = function(captures, arguments)
    local A, B, J, K, Q, X, a, branch_result, g, l, n, o, u, y
    local e7 = {}
    local steps = {}
    steps.entry = function()
        do
            local temp_1, temp_2, temp_5, temp_6, temp_9
            temp_1 = captures[1].value
            temp_2 = arguments[1]
            temp_5 = temp_1.FOVRadius
            temp_6 = captures[4].value
            temp_9 = {ipairs(unpack_values({temp_6.GetPlayers(temp_6)}))}
            B, X, g, l, o, u = nil, temp_2, temp_9[1], temp_5, temp_9[3], temp_9[2]
        end
        return "step_2"
    end
    steps.step_2 = function()
        do
            local temp_1, temp_2
            temp_1, temp_2 = g(u, o)
            branch_result = temp_1
            a, o = temp_2, temp_1
        end
        if (not branch_result) then
            return nil, pack_values(unpack_values({B}))
        end
        do
            local temp_2
            temp_2 = (a ~= captures[5].value)
            branch_result = temp_2
            J = temp_2
        end
        if branch_result then
            do
                local temp_3
                temp_3 = a.Character
                branch_result = temp_3
                A = temp_3
            end
            if branch_result then
                do
                    local temp_3, temp_7
                    temp_3 = a.Character
                    temp_7 = temp_3.FindFirstChild(temp_3, "Humanoid")
                    branch_result = temp_7
                    K = temp_7
                end
                if branch_result then
                    K = (a.Character.Humanoid.Health > 0)
                end
                A = K
            end
            J = A
        end
        branch_result = J
        if branch_result then
            return "step_3"
        else
            return "step_8"
        end
    end
    steps.step_3 = function()
        do
            local temp_5
            temp_5 = captures[6].value(a.Character)
            branch_result = temp_5
            J = temp_5
        end
        if branch_result then
            return "step_4"
        else
            return "step_7"
        end
    end
    steps.step_4 = function()
        do
            local temp_4
            temp_4 = captures[1].value.AimbotTeamCheck
            branch_result = temp_4
            A = temp_4
        end
        if branch_result then
            A = (not captures[7].value(a))
        end
        branch_result = A
        if branch_result then
            return "step_5"
        else
            return "step_6"
        end
    end
    steps.step_5 = function()

        return "step_2"
    end
    steps.step_6 = function()
        do
            local temp_1, temp_6, temp_7
            temp_1 = captures[8].value
            temp_6 = {temp_1.WorldToViewportPoint(temp_1, J.Position)}
            temp_7 = temp_6[1]
            branch_result = temp_6[2]
            A = temp_7
        end
        if branch_result then
            do
                local temp_14
                temp_14 = (Vector2.new(A.X, A.Y) - X).Magnitude
                branch_result = (temp_14 < l)
                K = temp_14
            end
            if branch_result then
                branch_result = captures[1].value.AimbotWallCheck
                if branch_result then
                    do
                        local temp_4, temp_44
                        temp_4 = RaycastParams.new()
                        temp_4.FilterDescendantsInstances = {captures[5].value.Character, captures[9].value}
                        temp_4.FilterType = Enum.RaycastFilterType.Exclude
                        temp_44 = workspace.Raycast(workspace, captures[8].value.CFrame.Position, ((J.Position - captures[8].value.CFrame.Position).Unit * 1000), temp_4)
                        branch_result = temp_44
                        n, y = temp_44, temp_44
                    end
                    if branch_result then
                        do
                            local temp_3
                            temp_3 = n.Instance
                            branch_result = temp_3
                            Q = temp_3
                        end
                        if branch_result then
                            do
                                local temp_3
                                temp_3 = n.Instance
                                Q = temp_3.IsDescendantOf(temp_3, a.Character)
                            end
                        end
                        y = Q
                    end
                    branch_result = y
                    if branch_result then
                        B, l = a, K
                    end
                else
                    B, l = a, K
                end
            end
        end
        return "step_7"
    end
    steps.step_7 = function()

        return "step_8"
    end
    steps.step_8 = function()

        return "step_2"
    end
    local current_step, result = "entry", nil
    while current_step do
        current_step, result = steps[current_step]()
    end
    if result then
        return unpack_values(result, 1, result.n)
    end
end

callbacks.next_decryption_byte = function(captures, arguments)
    local branch_result
    local e7 = {}
    branch_result = ((# captures[1].value) == 0)
    if branch_result then
        captures[2].value = (((captures[2].value * 213) + 27552854471197) % 35184372088832)
        while true do
            captures[3].value = ((captures[3].value * 201) % 257)
            branch_result = (captures[3].value ~= 1)
            if not (not branch_result) then break end
        end
        do
            local temp_14, temp_2, temp_21, temp_22, temp_24
            temp_2 = (captures[3].value % 32)
            temp_14 = ((captures[4].value((captures[2].value / (2 ^ (13 - ((captures[3].value - temp_2) / 32))))) % 4294967296) / (2 ^ temp_2))
            temp_21 = (captures[4].value(((temp_14 % 1) * 4294967296)) + captures[4].value(temp_14))
            temp_22 = (temp_21 % 65536)
            temp_24 = ((temp_21 - temp_22) / 65536)
            captures[1].value = {(temp_22 % 256), ((temp_22 - (temp_22 % 256)) / 256), (temp_24 % 256), ((temp_24 - (temp_24 % 256)) / 256)}
        end
    end
    do
        local temp_2, temp_4
        temp_2 = (# captures[1].value)
        temp_4 = captures[1].value[temp_2]
        captures[1].value[temp_2] = nil
        return unpack_values({temp_4})
    end
end

callbacks.decode_cached_string = function(captures, arguments)
    local A, B, X, a, branch_result, l, q, u, z
    local e7 = {}
    do
        local temp_1, temp_2, temp_3
        temp_1 = arguments[1]
        temp_2 = arguments[2]
        temp_3 = captures[1].value
        branch_result = temp_3[temp_2]
        B, X, l = temp_2, temp_1, temp_3
    end
    if branch_result then
        return unpack_values({B})
    end
    do
        local temp_2, temp_6
        captures[2].value = {}
        temp_2 = captures[3].value
        captures[4].value = (B % 35184372088832)
        captures[5].value = ((B % 255) + 2)
        temp_6 = (# X)
        l[B] = ""
        A, a, q, u, z = temp_6, "", 204, temp_2, 0
    end
    while true do
        do
            local temp_1
            temp_1 = (1 + z)
            branch_result = ((false and (temp_1 >= A)) or ((not false) and (A >= temp_1)))
            z = temp_1
        end
        if not branch_result then break end
        do
            local temp_7
            temp_7 = (((string.byte(X, z) + captures[6].value()) + q) % 256)
            a, q = (a .. u[(temp_7 + 1)]), temp_7
        end
    end
    l[B] = a
    return unpack_values({B})
end

callbacks.callback_80 = function(captures, arguments)
    local X, branch_result, l
    local e7 = {}
    do
        local temp_1, temp_2, temp_6
        temp_1 = arguments[2]
        temp_2 = arguments[1]
        temp_6 = RaycastParams.new()
        temp_6.FilterType = Enum.RaycastFilterType.Exclude
        temp_6.FilterDescendantsInstances = captures[3].value(temp_1)
        branch_result = workspace.Raycast(workspace, captures[4].value.CFrame.Position, (temp_2.Position - captures[4].value.CFrame.Position), temp_6)
        X, l = temp_2, temp_6
    end
    if branch_result then
        return unpack_values({false})
    end
    branch_result = workspace.Raycast(workspace, captures[5].value.Position, (X.Position - captures[5].value.Position), l)
    if branch_result then
        return unpack_values({false})
    end
    return unpack_values({true})
end

callbacks.callback_103 = function(captures, arguments)
    local branch_result
    local e7 = {}
    do
        local temp_1
        temp_1 = captures[1].value
        captures[2].value = temp_1.GetNetworkPing(temp_1)
        return unpack_values({})
    end
end

callbacks.callback_127 = function(captures, arguments)
    local branch_result
    local e7 = {}
    InstantKillTarget(captures[1].value.Parent)
    return unpack_values({})
end

callbacks.callback_144 = function(captures, arguments)
    local A, B, F, I, J, K, M, O, Q, S, V, W, X, a, branch_result, g, l, n, o, q, u, w, y, z
    local e7 = {}
    do
        local temp_1, temp_2, temp_3, temp_4
        temp_1 = arguments[3]
        temp_2 = arguments[1]
        temp_3 = arguments[2]
        temp_4 = (not temp_2)
        branch_result = temp_4
        B, I, X, l = temp_3, temp_4, temp_2, temp_1
    end
    if (not branch_result) then
        I = (not X.Parent)
    end
    branch_result = I
    if branch_result then
        return unpack_values({nil})
    end
    do
        local temp_4
        temp_4 = captures[3].value.Character
        branch_result = temp_4
        I, u = temp_4, temp_4
    end
    if branch_result then
        I = u.FindFirstChild(u, "HumanoidRootPart")
    end
    do
        local temp_3, temp_7
        temp_3 = X.Parent
        temp_7 = temp_3.FindFirstChild(temp_3, "HumanoidRootPart")
        branch_result = temp_7
        I, g, o = temp_7, temp_3, I
    end
    if (not branch_result) then
        I = X
    end
    do
        local temp_4
        temp_4 = g.FindFirstChildOfClass(g, "Humanoid")
        branch_result = (not o)
        a, q = temp_4, I
    end
    if branch_result then
        return unpack_values({X.Position})
    end
    do
        local temp_10, temp_11, temp_12, temp_16, temp_26, temp_3, temp_4, temp_7
        temp_3 = g.Name
        temp_4 = tick()
        temp_7 = X.Position
        temp_10 = {}
        temp_11 = q.AssemblyLinearVelocity
        temp_12 = bind(callbacks.callback_199, {captures[3], captures[1], captures[2], temp_10, captures[4]})
        temp_10.value = 0.03
        pcall(temp_12)
        temp_16 = ((temp_10.value / 2) + 0.016)
        temp_26 = (q.Position - o.Position).Magnitude
        branch_result = (not l)
        A, J, K, M, n, y, z = temp_4, temp_3, temp_11, temp_16, temp_26, temp_16, temp_7
    end
    if branch_result then
        branch_result = B
        S = B
        if branch_result then
            S = (B > 0)
        end
        branch_result = S
        Q = S
        if branch_result then
            Q = B
        end
        y = ((n / (Q or 1200)) + M)
    end
    do
        local temp_2
        temp_2 = captures[5].value[J]
        branch_result = temp_2
        Q = temp_2
    end
    if (not branch_result) then
        Q = {["lastVel"] = K, ["lastTime"] = A}
    end
    do
        local temp_13
        temp_13 = ((K - Q.lastVel) / math.max((A - Q.lastTime), 0.001))
        branch_result = (K.Dot(K, Q.lastVel) < 0)
        V = temp_13
    end
    if branch_result then
        V = Vector3.zero
    end
    do
        local temp_12
        captures[5].value[J] = {["lastVel"] = K, ["lastTime"] = A}
        temp_12 = ((z + (K * y)) + ((0.5 * V) * (y ^ 2)))
        branch_result = a
        W, w = temp_12, a
    end
    if branch_result then
        do
            local temp_10
            temp_10 = (a.FloorMaterial == Enum.Material.Air)
            branch_result = temp_10
            F = temp_10
        end
        if (not branch_result) then
            do
                local temp_9
                temp_9 = (a.GetState(a) == Enum.HumanoidStateType.Freefall)
                branch_result = temp_9
                O = temp_9
            end
            if (not branch_result) then
                O = (a.GetState(a) == Enum.HumanoidStateType.Jumping)
            end
            F = O
        end
        w = F
    end
    branch_result = w
    if branch_result then
        W = Vector3.new(W.X, ((z.Y + (K.Y * y)) - ((0.5 * (workspace.Gravity or 196.2)) * (y ^ 2))), W.Z)
    end
    do
        local temp_18, temp_4
        temp_4 = RaycastParams.new()
        temp_4.FilterType = Enum.RaycastFilterType.Exclude
        temp_4.FilterDescendantsInstances = {g, u}
        temp_18 = workspace.Raycast(workspace, z, (W - z), temp_4)
        branch_result = temp_18
        F = temp_18
    end
    if branch_result then
        W = (F.Position - ((W - z).Unit * 0.3))
    end
    return unpack_values({W})
end

callbacks.callback_145 = function(captures, arguments)
    local A, B, D, I, J, K, M, X, a, branch_result, g, l, n, o, q, u, y
    local e7 = {}
    local steps = {}
    steps.entry = function()
        do
            local temp_1, temp_2
            temp_1 = arguments[1]
            temp_2 = (not temp_1)
            branch_result = temp_2
            I, X = temp_2, temp_1
        end
        if (not branch_result) then
            I = (X == captures[1].value)
        end
        branch_result = I
        if branch_result then
            return nil, pack_values(unpack_values({false}))
        end
        branch_result = (not captures[2].value.AimbotTeamCheck)
        if branch_result then
            return nil, pack_values(unpack_values({true}))
        end
        do
            local temp_4
            temp_4 = captures[1].value.Team
            branch_result = temp_4
            I = temp_4
        end
        if branch_result then
            I = X.Team
        end
        branch_result = I
        if branch_result then
            return nil, pack_values(unpack_values({(captures[1].value.Team ~= X.Team)}))
        end
        do
            local temp_3
            temp_3 = X.Character
            branch_result = (not temp_3)
            B = temp_3
        end
        if branch_result then
            return nil, pack_values(unpack_values({false}))
        end
        do
            local temp_1, temp_3
            temp_1 = tick()
            temp_3 = captures[5].value[X]
            branch_result = temp_3
            I, l = temp_3, temp_1
        end
        if branch_result then
            I = ((l - captures[5].value[X].lastCheck) < captures[6].value)
        end
        branch_result = I
        if branch_result then
            return nil, pack_values(unpack_values({captures[5].value[X].isEnemy}))
        end
        do
            local temp_5
            temp_5 = B.FindFirstChild(B, "HumanoidRootPart")
            branch_result = temp_5
            g, o, q, u = temp_5, temp_5, temp_5, true
        end
        if branch_result then
            g = o.FindFirstChild(o, "TeammateIconPart")
        end
        branch_result = g
        I = g
        if (not branch_result) then
            I = B.FindFirstChild(B, "TeammateIconPart")
        end
        branch_result = I
        if branch_result then
            return "step_2"
        else
            return "step_4"
        end
    end
    steps.step_2 = function()
        u = false
        return "step_3"
    end
    steps.step_3 = function()
        do
            local temp_1, temp_6
            temp_1 = captures[5].value
            temp_6 = {u}
            temp_1[X] = {["isEnemy"] = u, ["lastCheck"] = l}
            return nil, pack_values(unpack_values(temp_6))
        end
    end
    steps.step_4 = function()
        do
            local temp_4
            temp_4 = B.FindFirstChild(B, "Head")
            branch_result = temp_4
            a, g = temp_4, temp_4
        end
        if branch_result then
            a = g.FindFirstChildOfClass(g, "BillboardGui")
        end
        branch_result = a
        I = a
        if (not branch_result) then
            I = B.FindFirstChildOfClass(B, "BillboardGui")
        end
        branch_result = I
        q = I
        if branch_result then
            return "step_5"
        else
            return "step_11"
        end
    end
    steps.step_5 = function()
        do
            local temp_3
            temp_3 = {ipairs(unpack_values({q.GetDescendants(q)}))}
            A, J, a = temp_3[3], temp_3[2], temp_3[1]
        end
        return "step_6"
    end
    steps.step_6 = function()
        do
            local temp_1, temp_2
            temp_1, temp_2 = a(J, A)
            branch_result = temp_1
            A, K = temp_1, temp_2
        end
        if branch_result then
            return "step_7"
        else
            return "step_10"
        end
    end
    steps.step_7 = function()
        do
            local temp_4
            temp_4 = K.IsA(K, "ImageLabel")
            branch_result = temp_4
            I = temp_4
        end
        if (not branch_result) then
            I = K.IsA(K, "ImageButton")
        end
        branch_result = I
        if branch_result then
            return "step_8"
        else
            return "step_14"
        end
    end
    steps.step_8 = function()
        do
            local temp_3
            temp_3 = K.ImageColor3
            branch_result = temp_3
            D, I = temp_3, temp_3
        end
        if branch_result then
            do
                local temp_4
                temp_4 = (D.G > 0.6)
                branch_result = temp_4
                n = temp_4
            end
            if branch_result then
                n = (D.R < 0.5)
            end
            I = n
        end
        branch_result = I
        if branch_result then
            return "step_9"
        else
            return "step_12"
        end
    end
    steps.step_9 = function()
        u = false
        return "step_10"
    end
    steps.step_10 = function()

        return "step_11"
    end
    steps.step_11 = function()

        return "step_3"
    end
    steps.step_12 = function()

        return "step_13"
    end
    steps.step_13 = function()

        return "step_6"
    end
    steps.step_14 = function()
        branch_result = K.IsA(K, "Frame")
        if branch_result then
            return "step_15"
        else
            return "step_13"
        end
    end
    steps.step_15 = function()
        do
            local temp_3
            temp_3 = K.BackgroundColor3
            branch_result = temp_3
            D, M = temp_3, temp_3
        end
        if branch_result then
            do
                local temp_4
                temp_4 = (D.G > 0.6)
                branch_result = temp_4
                y = temp_4
            end
            if branch_result then
                y = (D.R < 0.5)
            end
            M = y
        end
        branch_result = M
        if branch_result then
            return "step_16"
        else
            return "step_17"
        end
    end
    steps.step_16 = function()
        u = false
        return "step_10"
    end
    steps.step_17 = function()

        return "step_13"
    end
    local current_step, result = "entry", nil
    while current_step do
        current_step, result = steps[current_step]()
    end
    if result then
        return unpack_values(result, 1, result.n)
    end
end

callbacks.callback_149 = function(captures, arguments)
    local branch_result
    local e7 = {}
    return unpack_values({unpack_values({captures[1].value(arguments[1])})})
end

callbacks.callback_150 = function(captures, arguments)
    local B, I, X, branch_result, l, o, q, u
    local e7 = {}
    do
        local temp_4
        temp_4 = captures[1].value.Character
        branch_result = (not temp_4)
        X = temp_4
    end
    if branch_result then
        return unpack_values({true})
    end
    branch_result = X.FindFirstChildOfClass(X, "ForceField")
    if branch_result then
        return unpack_values({true})
    end
    branch_result = captures[1].value.Team
    if branch_result then
        do
            local temp_11, temp_17
            temp_11 = string.lower(captures[1].value.Team.Name)
            temp_17 = string.find(temp_11, "lobby")
            branch_result = temp_17
            B, I = temp_11, temp_17
        end
        if (not branch_result) then
            do
                local temp_6
                temp_6 = string.find(B, "spectat")
                branch_result = temp_6
                l = temp_6
            end
            if (not branch_result) then
                do
                    local temp_6
                    temp_6 = string.find(B, "espectador")
                    branch_result = temp_6
                    o = temp_6
                end
                if (not branch_result) then
                    do
                        local temp_6
                        temp_6 = string.find(B, "menu")
                        branch_result = temp_6
                        q = temp_6
                    end
                    if (not branch_result) then
                        q = string.find(B, "dead")
                    end
                    o = q
                end
                l = o
            end
            I = l
        end
        branch_result = I
        if branch_result then
            return unpack_values({true})
        end
    end
    do
        local temp_4
        temp_4 = X.FindFirstChild(X, "HumanoidRootPart")
        branch_result = temp_4
        B = temp_4
    end
    if branch_result then
        do
            local temp_2
            temp_2 = {ipairs(captures[4].value)}
            l, o, u = temp_2[2], temp_2[1], temp_2[3]
        end
        while true do
            do
                local temp_1, temp_2
                temp_1, temp_2 = o(l, u)
                branch_result = temp_1
                q, u = temp_2, temp_1
            end
            if not branch_result then break end
            branch_result = ((B.Position - q.Centro).Magnitude <= q.Radio)
            if branch_result then
                return unpack_values({true})
            end
        end
    end
    return unpack_values({false})
end

callbacks.callback_151 = function(captures, arguments)
    local B, branch_result, l, o, q, u
    local e7 = {}
    do
        local temp_1, temp_3, temp_6
        temp_1 = {}
        temp_3 = captures[1].value
        temp_6 = {ipairs(unpack_values({temp_3.GetPlayers(temp_3)}))}
        B, l, o, u = temp_1, temp_6[2], temp_6[1], temp_6[3]
    end
    while true do
        do
            local temp_1, temp_2
            temp_1, temp_2 = o(l, u)
            branch_result = temp_1
            q, u = temp_2, temp_1
        end
        if (not branch_result) then
            return unpack_values({B})
        end
        branch_result = q.Character
        if branch_result then
            table.insert(B, q.Character)
        end
    end
end

callbacks.callback_152 = function(captures, arguments)
    local B, I, X, branch_result, u
    local e7 = {}
    do
        local temp_1
        temp_1 = arguments[1]
        branch_result = (not temp_1)
        X = temp_1
    end
    if branch_result then
        return unpack_values({false})
    end
    do
        local temp_4
        temp_4 = X.GetAttribute(X, "Downed")
        branch_result = temp_4
        I = temp_4
    end
    if (not branch_result) then
        do
            local temp_4
            temp_4 = X.GetAttribute(X, "Knocked")
            branch_result = temp_4
            B = temp_4
        end
        if (not branch_result) then
            do
                local temp_4
                temp_4 = X.GetAttribute(X, "IsDowned")
                branch_result = temp_4
                u = temp_4
            end
            if (not branch_result) then
                u = X.GetAttribute(X, "KO")
            end
            B = u
        end
        I = B
    end
    branch_result = I
    if branch_result then
        return unpack_values({true})
    end
    do
        local temp_4
        temp_4 = X.FindFirstChild(X, "Downed")
        branch_result = temp_4
        I = temp_4
    end
    if (not branch_result) then
        do
            local temp_4
            temp_4 = X.FindFirstChild(X, "Knocked")
            branch_result = temp_4
            B = temp_4
        end
        if (not branch_result) then
            B = X.FindFirstChild(X, "KO")
        end
        I = B
    end
    branch_result = I
    if branch_result then
        return unpack_values({true})
    end
    do
        local temp_4
        temp_4 = X.FindFirstChildOfClass(X, "Humanoid")
        branch_result = temp_4
        B, I = temp_4, temp_4
    end
    if branch_result then
        I = (B.Health < 15)
    end
    branch_result = I
    if branch_result then
    
    end
    return unpack_values({false})
end

callbacks.callback_153 = function(captures, arguments)
    local B, I, X, a, branch_result, g, o, u
    local e7 = {}
    do
        local temp_1, temp_2
        temp_1 = arguments[1]
        temp_2 = (not temp_1)
        branch_result = temp_2
        I, X = temp_2, temp_1
    end
    if (not branch_result) then
        I = (not X.IsA(X, "Tool"))
    end
    branch_result = I
    if branch_result then
        return unpack_values({false})
    end
    do
        local temp_5
        temp_5 = X.FindFirstChild(X, "Throw", true)
        branch_result = temp_5
        I = temp_5
    end
    if (not branch_result) then
        do
            local temp_5
            temp_5 = X.FindFirstChild(X, "KnifeClient", true)
            branch_result = temp_5
            B = temp_5
        end
        if (not branch_result) then
            do
                local temp_5
                temp_5 = X.FindFirstChild(X, "KnifeServer", true)
                branch_result = temp_5
                u = temp_5
            end
            if (not branch_result) then
                u = X.FindFirstChild(X, "ThrowingKinfeHandlerScript", true)
            end
            B = u
        end
        I = B
    end
    branch_result = I
    if branch_result then
        return unpack_values({false})
    end
    do
        local temp_51, temp_7
        temp_7 = string.lower(X.Name)
        temp_51 = {ipairs({"combat", "fist", "wallet", "phone", "punch", "boombox", "radio", "knife", "blade", "cuchillo", "dagger", "kunai", "sword", "toy", "juguete", "pizza", "burger", "teddy", "balloon", "drink", "food"})}
        B, g, o, u = temp_7, temp_51[1], temp_51[3], temp_51[2]
    end
    while true do
        do
            local temp_1, temp_2
            temp_1, temp_2 = g(u, o)
            branch_result = temp_1
            a, o = temp_2, temp_1
        end
        if (not branch_result) then
            return unpack_values({true})
        end
        branch_result = string.find(B, a)
        if not (not branch_result) then break end
    
    end
    return unpack_values({false})
end

callbacks.callback_154 = function(captures, arguments)
    local A, B, I, J, X, branch_result, g, l, o, q, u
    local e7 = {}
    do
        local temp_4
        temp_4 = captures[1].value.Character
        branch_result = (not temp_4)
        X = temp_4
    end
    if branch_result then
        return unpack_values({nil})
    end
    do
        local temp_4
        temp_4 = X.FindFirstChildOfClass(X, "Humanoid")
        branch_result = (not temp_4)
        B = temp_4
    end
    if branch_result then
        return unpack_values({nil})
    end
    do
        local temp_4
        temp_4 = X.FindFirstChildOfClass(X, "Tool")
        branch_result = temp_4
        I, l = temp_4, temp_4
    end
    if branch_result then
        I = captures[4].value(l)
    end
    branch_result = I
    if branch_result then
        return unpack_values({l})
    end
    do
        local temp_1, temp_5
        temp_1 = captures[1].value
        temp_5 = temp_1.FindFirstChild(temp_1, "Backpack")
        branch_result = temp_5
        u = temp_5
    end
    if branch_result then
        do
            local temp_3
            temp_3 = {ipairs(unpack_values({u.GetChildren(u)}))}
            g, o, q = temp_3[3], temp_3[2], temp_3[1]
        end
        while true do
            do
                local temp_1, temp_2
                temp_1, temp_2 = q(o, g)
                branch_result = temp_1
                J, g = temp_2, temp_1
            end
            if not branch_result then break end
            do
                local temp_4
                temp_4 = J.IsA(J, "Tool")
                branch_result = temp_4
                A = temp_4
            end
            if branch_result then
                A = captures[4].value(J)
            end
            branch_result = A
            if branch_result then
                do
                    local temp_1
                    temp_1 = {J}
                    B.EquipTool(B, J)
                    task.wait(0.04)
                    return unpack_values(temp_1)
                end
            end
        end
    end
    return unpack_values({nil})
end

callbacks.callback_155 = function(captures, arguments)
    local A, B, D, I, J, K, M, Q, S, X, a, branch_result, g, l, n, o, q, u, y, z
    local e7 = {}
    do
        local temp_3, temp_4, temp_8
        temp_3 = workspace.CurrentCamera
        temp_4 = {}
        temp_4.value = temp_3
        temp_8 = captures[3].value.Character
        branch_result = temp_8
        B, I, X = temp_8, temp_8, temp_4
    end
    if branch_result then
        I = B.FindFirstChild(B, "Head")
    end
    do
        local temp_1, temp_3
        temp_1 = {}
        temp_1.value = I
        temp_3 = (not X.value)
        branch_result = temp_3
        I, l = temp_3, temp_1
    end
    if (not branch_result) then
        do
            local temp_1
            temp_1 = (not B)
            branch_result = temp_1
            u = temp_1
        end
        if (not branch_result) then
            u = (not l.value)
        end
        I = u
    end
    branch_result = I
    if branch_result then
        return unpack_values({nil})
    end
    do
        local temp_1, temp_2
        temp_1 = bind(callbacks.callback_80, {captures[1], captures[2], captures[4], X, l})
        temp_2 = captures[5].value
        branch_result = temp_2
        I, o = temp_2, temp_1
    end
    if branch_result then
        I = captures[5].value.Parent
    end
    branch_result = I
    if branch_result then
        do
            local temp_11, temp_4, temp_8, temp_9
            temp_4 = captures[5].value.Parent
            temp_8 = temp_4.FindFirstChildOfClass(temp_4, "Humanoid")
            temp_9 = captures[6].value
            temp_11 = temp_9.GetPlayerFromCharacter(temp_9, temp_4)
            branch_result = temp_8
            I, a, g, q = temp_8, temp_11, temp_4, temp_8
        end
        if branch_result then
            do
                local temp_4
                temp_4 = (q.Health > 0)
                branch_result = temp_4
                J = temp_4
            end
            if branch_result then
                branch_result = a
                z = a
                if branch_result then
                    do
                        local temp_2
                        temp_2 = captures[7].value(a)
                        branch_result = temp_2
                        D = temp_2
                    end
                    if branch_result then
                        D = (not captures[8].value(g))
                    end
                    z = D
                end
                J = z
            end
            I = J
        end
        branch_result = I
        if branch_result then
            branch_result = ((l.value.Position - captures[5].value.Position).Magnitude <= 350)
            if branch_result then
                branch_result = o(captures[5].value, g)
                if branch_result then
                    return unpack_values({captures[5].value})
                end
            end
        end
    end
    do
        local temp_3, temp_4, temp_7
        temp_3 = math.huge
        temp_4 = captures[6].value
        temp_7 = {ipairs(unpack_values({temp_4.GetPlayers(temp_4)}))}
        A, J, a, g, q = temp_7[1], temp_7[3], temp_7[2], nil, temp_3
    end
    while true do
        do
            local temp_1, temp_2
            temp_1, temp_2 = A(a, J)
            branch_result = temp_1
            J, K = temp_1, temp_2
        end
        if (not branch_result) then
            do
                local temp_1
                temp_1 = {g}
                captures[5].value = g
                return unpack_values(temp_1)
            end
        end
        do
            local temp_2
            temp_2 = (K ~= captures[3].value)
            branch_result = temp_2
            D = temp_2
        end
        if branch_result then
            do
                local temp_2
                temp_2 = captures[7].value(K)
                branch_result = temp_2
                M = temp_2
            end
            if branch_result then
                M = K.Character
            end
            D = M
        end
        branch_result = D
        if branch_result then
            do
                local temp_11, temp_3, temp_7
                temp_3 = K.Character
                temp_7 = temp_3.FindFirstChildOfClass(temp_3, "Humanoid")
                temp_11 = temp_3.FindFirstChild(temp_3, "Head")
                branch_result = temp_11
                D, M, n = temp_3, temp_7, temp_11
            end
            if (not branch_result) then
                n = D.FindFirstChild(D, "HumanoidRootPart")
            end
            branch_result = M
            y = M
            if branch_result then
                do
                    local temp_4
                    temp_4 = (M.Health > 0)
                    branch_result = temp_4
                    Q = temp_4
                end
                if branch_result then
                    do
                        local temp_3
                        temp_3 = (not captures[8].value(D))
                        branch_result = temp_3
                        S = temp_3
                    end
                    if branch_result then
                        S = n
                    end
                    Q = S
                end
                y = Q
            end
            branch_result = y
            if branch_result then
                do
                    local temp_11, temp_12
                    temp_11 = (l.value.Position - n.Position).Magnitude
                    temp_12 = (temp_11 <= 350)
                    branch_result = temp_12
                    Q, y = temp_12, temp_11
                end
                if branch_result then
                    Q = (y < q)
                end
                branch_result = Q
                if branch_result then
                    branch_result = o(n, D)
                    if branch_result then
                        g, q = n, y
                    end
                end
            end
        end
    end
end

callbacks.callback_156 = function(captures, arguments)
    local B, I, X, branch_result
    local e7 = {}
    do
        local temp_4
        temp_4 = captures[1].value.Character
        branch_result = temp_4
        I, X = temp_4, temp_4
    end
    if branch_result then
        I = X.FindFirstChildOfClass(X, "Humanoid")
    end
    branch_result = I
    B = I
    if branch_result then
        B.UnequipTools(B)
    end
    return unpack_values({})
end

callbacks.callback_157 = function(captures, arguments)
    local B, I, Q, X, a, branch_result, g, l, n, o, q, u, y
    local e7 = {}
    local steps = {}
    steps.entry = function()

        return "step_2"
    end
    steps.step_2 = function()
        branch_result = task.wait(0.03)
        if (not branch_result) then
            return nil, pack_values(unpack_values({}))
        end
        do
            local temp_1, temp_2, temp_4
            temp_1 = captures[3].value
            temp_2 = captures[1].value
            temp_4 = temp_1.AutoShootEnabled
            branch_result = temp_4
            I, u = temp_4, temp_2
        end
        if branch_result then
            do
                local temp_1, temp_4
                temp_1 = captures[3].value
                temp_4 = temp_1.ScriptActive
                branch_result = temp_4
                X, u = temp_4, temp_1
            end
            if branch_result then
                do
                    local temp_1
                    temp_1 = captures[4].value
                    X, u = (not temp_1()), temp_1
                end
            end
            I = X
        end
        branch_result = I
        if branch_result then
            return "step_3"
        else
            return "step_9"
        end
    end
    steps.step_3 = function()
        branch_result = ((tick() - captures[5].value) < captures[6].value)
        if branch_result then
            return "step_4"
        else
            return "step_5"
        end
    end
    steps.step_4 = function()
        captures[7].value()
        getgenv().KaiserTargetPart = nil
        return "step_2"
    end
    steps.step_5 = function()
        do
            local temp_4
            temp_4 = captures[8].value.Character
            branch_result = temp_4
            B, I = temp_4, temp_4
        end
        if branch_result then
            I = B.FindFirstChild(B, "HumanoidRootPart")
        end
        do
            local temp_1
            temp_1 = (not B)
            branch_result = temp_1
            I, l = temp_1, I
        end
        if (not branch_result) then
            I = (not l)
        end
        branch_result = I
        if branch_result then
            return "step_6"
        else
            return "step_7"
        end
    end
    steps.step_6 = function()

        return "step_2"
    end
    steps.step_7 = function()
        do
            local temp_1
            temp_1 = {}
            temp_1.value = captures[9].value()
            branch_result = temp_1.value
            u = temp_1
        end
        if branch_result then
            do
                local temp_2
                temp_2 = captures[10].value()
                branch_result = temp_2
                o = temp_2
            end
            if branch_result then
                do
                    local temp_7
                    getgenv().KaiserTargetPart = u.value
                    temp_7 = workspace.CurrentCamera
                    branch_result = temp_7
                    g = temp_7
                end
                if branch_result then
                    do
                        local temp_10, temp_4, temp_7
                        temp_4 = g.CFrame
                        temp_7 = l.CFrame
                        temp_10 = captures[11].value(u.value, 2000, true)
                        branch_result = temp_10
                        I, a, q = temp_10, temp_7, temp_4
                    end
                    if (not branch_result) then
                        I = u.value.Position
                    end
                    do
                        local temp_1, temp_11, temp_14, temp_26, temp_3
                        temp_1 = captures[12].value
                        temp_3 = temp_1.GetMouseLocation(temp_1)
                        temp_11 = g.ViewportPointToRay(g, temp_3.X, temp_3.Y)
                        temp_14 = g.CFrame
                        temp_26 = CFrame.lookAt(Vector3.zero, temp_14.VectorToObjectSpace(temp_14, temp_11.Direction))
                        Q, n, y = 0, (CFrame.lookAt(g.CFrame.Position, (g.CFrame.Position + (I - g.CFrame.Position).Unit)) * temp_26.Inverse(temp_26)), Vector3.new(I.X, l.Position.Y, I.Z)
                    end
                    while true do
                        do
                            local temp_1
                            temp_1 = (1 + Q)
                            branch_result = ((false and (temp_1 >= 2)) or ((not false) and (2 >= temp_1)))
                            Q = temp_1
                        end
                        if not branch_result then break end
                        g.CFrame = n
                        l.CFrame = CFrame.lookAt(l.Position, y)
                        branch_result = (Q == 1)
                        if branch_result then
                            o.Activate(o)
                            branch_result = u.value.Parent
                            if branch_result then
                                pcall(bind(callbacks.callback_127, {u, captures[1], captures[2]}))
                            end
                        end
                        do
                            local temp_4
                            temp_4 = captures[13].value.RenderStepped
                            temp_4.Wait(temp_4)
                        end
                    end
                    captures[5].value = tick()
                    branch_result = (not captures[3].value.AimbotEnabled)
                    if branch_result then
                        g.CFrame = q
                        l.CFrame = a
                    end
                    task.wait(0.04)
                    captures[7].value()
                end
            end
        else
            getgenv().KaiserTargetPart = nil
        end
        return "step_8"
    end
    steps.step_8 = function()

        return "step_2"
    end
    steps.step_9 = function()
        getgenv().KaiserTargetPart = nil
        return "step_8"
    end
    local current_step, result = "entry", nil
    while current_step do
        current_step, result = steps[current_step]()
    end
    if result then
        return unpack_values(result, 1, result.n)
    end
end

callbacks.callback_199 = function(captures, arguments)
    local B, I, X, branch_result, l
    local e7 = {}
    branch_result = captures[1].value.GetNetworkPing
    if branch_result then
        do
            local temp_1, temp_3, temp_4
            temp_1 = captures[1].value
            temp_3 = temp_1.GetNetworkPing(temp_1)
            temp_4 = (temp_3 > 5)
            branch_result = temp_4
            B, X, l = temp_4, temp_3, temp_1
        end
        if branch_result then
            B = (X / 1000)
        end
        branch_result = B
        I = B
        if (not branch_result) then
            I = X
        end
        captures[4].value = I
    else
        do
            local temp_1
            temp_1 = captures[5].value
            branch_result = temp_1
            X = temp_1
        end
        if branch_result then
            do
                local temp_1, temp_5
                temp_1 = captures[5].value
                temp_5 = temp_1.FindFirstChild(temp_1, "Network")
                branch_result = temp_5
                B = temp_5
            end
            if branch_result then
                do
                    local temp_4
                    temp_4 = captures[5].value.Network
                    B = temp_4.FindFirstChild(temp_4, "ServerStatsItem")
                end
            end
            X = B
        end
        branch_result = X
        if branch_result then
            do
                local temp_10
                temp_10 = captures[5].value.Network.ServerStatsItem["Data Ping"]
                captures[4].value = (temp_10.GetValue(temp_10) / 1000)
            end
        end
    end
    return unpack_values({})
end

-- ENSAMBLADO DE LAS DEPENDENCIAS DEL AIMBOT Y AUTOSHOOT.
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Stats = game:GetService("Stats")
local localPlayer = cell(Players.LocalPlayer)
local players = cell(Players)
local runService = cell(RunService)
local inputService = cell(UserInputService)
local statistics = cell(Stats)
local settings = cell(SETTINGS)
local camera = cell(workspace.CurrentCamera)
local ignoreInstance = cell(nil)

-- Soporte de las referencias de cadenas conservadas por los callbacks originales.
local characters = cell({})
for byte = 0, 255 do characters.value[byte + 1] = string.char(byte) end
local byteBuffer, stringCache = cell({}), cell({})
local seed35, seed257, floor = cell(0), cell(2), cell(math.floor)
local nextByte = cell(bind(callbacks.next_decryption_byte, {byteBuffer, seed35, seed257, floor}))
local decodeString = cell(bind(callbacks.decode_cached_string,
    {stringCache, byteBuffer, characters, seed35, seed257, nextByte}))

local getAimPart = cell(bind(callbacks.callback_55, {settings, stringCache, decodeString}))
local enemyCache, enemyCacheInterval = cell({}), cell(0.5)
local isEnemy = cell(bind(callbacks.callback_145,
    {localPlayer, settings, stringCache, decodeString, enemyCache, enemyCacheInterval}))
local nearestPlayer = bind(callbacks.callback_56,
    {settings, stringCache, decodeString, players, localPlayer, getAimPart, isEnemy, camera, ignoreInstance})
local velocityHistory = cell({})
local predictPosition = cell(bind(callbacks.callback_144,
    {stringCache, decodeString, localPlayer, statistics, velocityHistory}))
local safeZone = cell(bind(callbacks.callback_150,
    {localPlayer, stringCache, decodeString, cell(SAFE_ZONES)}))
local allCharacters = cell(bind(callbacks.callback_151, {players, stringCache, decodeString}))
local isDowned = cell(bind(callbacks.callback_152, {stringCache, decodeString}))
local isGun = cell(bind(callbacks.callback_153, {stringCache, decodeString}))
local equipGun = cell(bind(callbacks.callback_154, {localPlayer, stringCache, decodeString, isGun}))
local lastTarget = cell(nil)
local enemyCheck = cell(bind(callbacks.callback_149, {isEnemy}))
local findShotTarget = cell(bind(callbacks.callback_155,
    {stringCache, decodeString, localPlayer, allCharacters, lastTarget, players, enemyCheck, isDowned}))
local unequipTools = cell(bind(callbacks.callback_156, {localPlayer, stringCache, decodeString}))
local lastShot, interval = cell(0), cell(AUTO_SHOOT_INTERVAL)
local autoShoot = bind(callbacks.callback_157,
    {stringCache, decodeString, settings, safeZone, lastShot, interval, unequipTools,
     localPlayer, findShotTarget, equipGun, predictPosition, inputService, runService})

local Controller = {Settings = SETTINGS}
local frameConnection, autoShootThread

function Controller.UpdateCameraAim()
    camera.value = workspace.CurrentCamera
    if not SETTINGS.ScriptActive or not SETTINGS.AimbotEnabled or not camera.value then return end
    local origin
    if SETTINGS.FOVIsMobile then
        origin = camera.value.ViewportSize / 2
    else
        origin = UserInputService:GetMouseLocation()
    end
    local target = nearestPlayer(origin)
    if target and target.Character then
        local part = getAimPart.value(target.Character)
        if part then
            camera.value.CFrame = CFrame.new(camera.value.CFrame.Position, part.Position)
        end
    end
end

function Controller.Start()
    if frameConnection then return end
    SETTINGS.ScriptActive = true
    frameConnection = RunService.RenderStepped:Connect(Controller.UpdateCameraAim)
    autoShootThread = task.spawn(autoShoot)
end

function Controller.Stop()
    SETTINGS.ScriptActive = false
    if frameConnection then frameConnection:Disconnect(); frameConnection = nil end
    if autoShootThread then
        pcall(task.cancel, autoShootThread)
        autoShootThread = nil
    end
    getgenv().KaiserTargetPart = nil
end

Controller.FindAimPart = getAimPart.value
Controller.FindShotTarget = findShotTarget.value
Controller.IsEnemy = isEnemy.value
Controller.Start()
return Controller
