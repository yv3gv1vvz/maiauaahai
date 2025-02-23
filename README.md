end 
 
function QuaternionFromCFrame(cf) 
local mx, my, mz, m00, m01, m02, m10, m11, m12, m20, m21, m22 = cf:components() 
local trace = m00 + m11 + m22 
if trace > 0 then 
local s = math.sqrt(1 + trace) 
local recip = 0.5/s 
return (m21-m12)*recip, (m02-m20)*recip, (m10-m01)*recip, s*0.5 
else 
local i = 0 
if m11 > m00 then
i = 1
end
if m22 > (i == 0 and m00 or m11) then 
i = 2 
end 
if i == 0 then 
local s = math.sqrt(m00-m11-m22+1) 
local recip = 0.5/s 
return 0.5*s, (m10+m01)*recip, (m20+m02)*recip, (m21-m12)*recip 
elseif i == 1 then 
local s = math.sqrt(m11-m22-m00+1) 
local recip = 0.5/s 
return (m01+m10)*recip, 0.5*s, (m21+m12)*recip, (m02-m20)*recip 
elseif i == 2 then 
local s = math.sqrt(m22-m00-m11+1) 
local recip = 0.5/s return (m02+m20)*recip, (m12+m21)*recip, 0.5*s, (m10-m01)*recip 
end 
end 
end
 
function QuaternionToCFrame(px, py, pz, x, y, z, w) 
local xs, ys, zs = x + x, y + y, z + z 
local wx, wy, wz = w*xs, w*ys, w*zs 
local xx = x*xs 
local xy = x*ys 
local xz = x*zs 
local yy = y*ys 
local yz = y*zs 
local zz = z*zs 
return CFrame.new(px, py, pz,1-(yy+zz), xy - wz, xz + wy,xy + wz, 1-(xx+zz), yz - wx, xz - wy, yz + wx, 1-(xx+yy)) 
end
 
function QuaternionSlerp(a, b, t) 
local cosTheta = a[1]*b[1] + a[2]*b[2] + a[3]*b[3] + a[4]*b[4] 
local startInterp, finishInterp; 
if cosTheta >= 0.0001 then 
if (1 - cosTheta) > 0.0001 then 
local theta = math.acos(cosTheta) 
local invSinTheta = 1/math.sin(theta) 
startInterp = math.sin((1-t)*theta)*invSinTheta 
finishInterp = math.sin(t*theta)*invSinTheta  
else 
startInterp = 1-t 
finishInterp = t 
end 
else 
if (1+cosTheta) > 0.0001 then 
local theta = math.acos(-cosTheta) 
local invSinTheta = 1/math.sin(theta) 
startInterp = math.sin((t-1)*theta)*invSinTheta 
finishInterp = math.sin(t*theta)*invSinTheta 
else 
startInterp = t-1 
finishInterp = t 
end 
end 
return a[1]*startInterp + b[1]*finishInterp, a[2]*startInterp + b[2]*finishInterp, a[3]*startInterp + b[3]*finishInterp, a[4]*startInterp + b[4]*finishInterp 
end

local function CFrameFromTopBack(at, top, back)
local right = top:Cross(back)
return CFrame.new(at.x, at.y, at.z,
right.x, top.x, back.x,
right.y, top.y, back.y,
right.z, top.z, back.z)
end

function Triangle(a, b, c)
local edg1 = (c-a):Dot((b-a).unit)
local edg2 = (a-b):Dot((c-b).unit)
local edg3 = (b-c):Dot((a-c).unit)
if edg1 <= (b-a).magnitude and edg1 >= 0 then
a, b, c = a, b, c
elseif edg2 <= (c-b).magnitude and edg2 >= 0 then
a, b, c = b, c, a
elseif edg3 <= (a-c).magnitude and edg3 >= 0 then
a, b, c = c, a, b
else
assert(false, "unreachable")
end
 
local len1 = (c-a):Dot((b-a).unit)
local len2 = (b-a).magnitude - len1
local width = (a + (b-a).unit*len1 - c).magnitude
 
local maincf = CFrameFromTopBack(a, (b-a):Cross(c-b).unit, -(b-a).unit)
 
local list = {}
 
if len1 > 0.01 then
local w1 = Instance.new('WedgePart', m)
game:GetService("Debris"):AddItem(w1,5)
w1.Material = "SmoothPlastic"
w1.FormFactor = 'Custom'
w1.BrickColor = BrickColor.new("Really red")
w1.Transparency = 0
w1.Reflectance = 0
w1.Material = "SmoothPlastic"
w1.CanCollide = false
local l1 = Instance.new("PointLight",w1)
l1.Color = Color3.new(170,0,0)
NoOutline(w1)
local sz = Vector3.new(0.2, width, len1)
w1.Size = sz
local sp = Instance.new("SpecialMesh",w1)
sp.MeshType = "Wedge"
sp.Scale = Vector3.new(0,1,1) * sz/w1.Size
w1:BreakJoints()
w1.Anchored = true
w1.Parent = workspace
w1.Transparency = 0.7
table.insert(Effects,{w1,"Disappear",.01})
w1.CFrame = maincf*CFrame.Angles(math.pi,0,math.pi/2)*CFrame.new(0,width/2,len1/2)
table.insert(list,w1)
end
 
if len2 > 0.01 then
local w2 = Instance.new('WedgePart', m)
game:GetService("Debris"):AddItem(w2,5)
w2.Material = "SmoothPlastic"
w2.FormFactor = 'Custom'
w2.BrickColor = BrickColor.new("Really red")
w2.Transparency = 0
w2.Reflectance = 0
w2.Material = "SmoothPlastic"
w2.CanCollide = false
local l2 = Instance.new("PointLight",w2)
l2.Color = Color3.new(170,0,0)
NoOutline(w2)
local sz = Vector3.new(0.2, width, len2)
w2.Size = sz
local sp = Instance.new("SpecialMesh",w2)
sp.MeshType = "Wedge"
sp.Scale = Vector3.new(0,1,1) * sz/w2.Size
w2:BreakJoints()
w2.Anchored = true
w2.Parent = workspace
w2.Transparency = 0.7
table.insert(Effects,{w2,"Disappear",.01})
w2.CFrame = maincf*CFrame.Angles(math.pi,math.pi,-math.pi/2)*CFrame.new(0,width/2,-len1 - len2/2)
table.insert(list,w2)
end
return unpack(list)
end
 

function Damagefunc(Part, hit, minim, maxim, knockback, Type, Property, Delay, HitSound, HitPitch)
  if hit.Parent == nil then
    return
  end
  local h = hit.Parent:FindFirstChildOfClass("Humanoid")
  for _, v in pairs(hit.Parent:children()) do
    if v:IsA("Humanoid") then
      h = v
    end
  end
  if h ~= nil and hit.Parent.Name ~= Character.Name and hit.Parent:FindFirstChild("Torso") ~= nil or h ~= nil and hit.Parent.Name ~= Character.Name and hit.Parent:FindFirstChild("UpperTorso") ~= nil then
    if hit.Parent:findFirstChild("DebounceHit") ~= nil and hit.Parent.DebounceHit.Value == true then
      return
    end
    local c = Create("ObjectValue")({
      Name = "creator",
      Value = game:service("Players").LocalPlayer,
      Parent = h
    })
    game:GetService("Debris"):AddItem(c, 0.5)
    if HitSound ~= nil and HitPitch ~= nil then
      CFuncs.Sound.Create(HitSound, hit, 1, HitPitch)
    end
    local Damage = math.random(minim, maxim)
    local blocked = false
    local block = hit.Parent:findFirstChild("Block")
    if block ~= nil and block.className == "IntValue" and block.Value > 0 then
      blocked = true
      block.Value = block.Value - 1
      print(block.Value)
    end
    if blocked == false then
	DamageFling(hit.Parent)
      if HitHealth ~= h.Health and HitHealth ~= 0 and 0 >= h.Health and h.Parent.Name ~= "Hologram" then
        print("gained kill")
      end
      ShowDamage(Part.CFrame * CFrame.new(0, 0, Part.Size.Z / 2).p + Vector3.new(0, 1.5, 0), -Damage, 1.5, Part.BrickColor.Color)
    else
	DamageFling(hit.Parent)
      ShowDamage(Part.CFrame * CFrame.new(0, 0, Part.Size.Z / 2).p + Vector3.new(0, 1.5, 0), -Damage, 1.5, Part.BrickColor.Color)
    end
    if Type == "Knockdown" then
      local hum = hit.Parent.Humanoid
      hum.PlatformStand = true
      coroutine.resume(coroutine.create(function(HHumanoid)

        HHumanoid.PlatformStand = true
      end), hum)
      local angle = hit.Position - (Property.Position + Vector3.new(0, 0, 0)).unit
      local bodvol = Create("BodyVelocity")({
        velocity = angle * knockback,
        P = 5000,
        maxForce = Vector3.new(8000, 8000, 8000),
        Parent = hit
      })
      local rl = Create("BodyAngularVelocity")({
        P = 3000,
        maxTorque = Vector3.new(500000, 500000, 500000) * 50000000000000,
        angularvelocity = Vector3.new(math.random(-10, 10), math.random(-10, 10), math.random(-10, 10)),
        Parent = hit
      })
      game:GetService("Debris"):AddItem(bodvol, 0.5)
      game:GetService("Debris"):AddItem(rl, 0.5)
    elseif Type == "Normal" then
      local vp = Create("BodyVelocity")({
        P = 500,
        maxForce = Vector3.new(math.huge, 0, math.huge),
        velocity = Property.CFrame.lookVector * knockback + Property.Velocity / 1.05
      })
      if knockback > 0 then
        vp.Parent = hit.Parent.Torso
      end
      game:GetService("Debris"):AddItem(vp, 0.5)
    elseif Type == "Up" then
      local bodyVelocity = Create("BodyVelocity")({
        velocity = Vector3.new(0, 20, 0),
        P = 5000,
        maxForce = Vector3.new(8000, 8000, 8000),
        Parent = hit
      })
      game:GetService("Debris"):AddItem(bodyVelocity, 0.5)
      local bodyVelocity = Create("BodyVelocity")({
        velocity = Vector3.new(0, 20, 0),
        P = 5000,
        maxForce = Vector3.new(8000, 8000, 8000),
        Parent = hit
      })
      game:GetService("Debris"):AddItem(bodyVelocity, 1)
    elseif Type == "Normal" then
      local hum = hit.Parent.Humanoid
      if hum ~= nil then
        for i = 0, 2 do
          Effects.Sphere.Create(BrickColor.new("Bright red"), hit.Parent.Torso.CFrame * cn(0, 0, 0) * angles(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50)), 1, 15, 1, 0, 5, 0, 0.02)
        end
      end
    elseif Type == "UpKnock" then
      local hum = hit.Parent.Humanoid
      hum.PlatformStand = true
      if hum ~= nil then
        hitr = true
      end
      coroutine.resume(coroutine.create(function(HHumanoid)

        HHumanoid.PlatformStand = false
        hitr = false
      end), hum)
      local bodyVelocity = Create("BodyVelocity")({
        velocity = Vector3.new(0, 20, 0),
        P = 5000,
        maxForce = Vector3.new(8000, 8000, 8000),
        Parent = hit
      })
      game:GetService("Debris"):AddItem(bodyVelocity, 0.5)
      local bodyVelocity = Create("BodyVelocity")({
        velocity = Vector3.new(0, 20, 0),
        P = 5000,
        maxForce = Vector3.new(8000, 8000, 8000),
        Parent = hit
      })
      game:GetService("Debris"):AddItem(bodyVelocity, 1)
    elseif Type == "Snare" then
      local bp = Create("BodyPosition")({
        P = 2000,
        D = 100,
        maxForce = Vector3.new(math.huge, math.huge, math.huge),
        position = hit.Parent.Torso.Position,
        Parent = hit.Parent.Torso
      })
      game:GetService("Debris"):AddItem(bp, 1)
    elseif Type == "Slashnare" then
      Effects.Block.Create(BrickColor.new("Pastel Blue"), hit.Parent.Torso.CFrame * cn(0, 0, 0), 15*4, 15*4, 15*4, 3*4, 3*4, 3*4, 0.07)
      for i = 1, math.random(4, 5) do
        Effects.Sphere.Create(BrickColor.new("Teal"), hit.Parent.Torso.CFrame * cn(math.random(-5, 5), math.random(-5, 5), math.random(-5, 5)) * angles(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50)), 1, 15, 1, 0, 5, 0, 0.02)
      end
      local bp = Create("BodyPosition")({
        P = 2000,
        D = 100,
        maxForce = Vector3.new(math.huge, math.huge, math.huge),
        position = hit.Parent.Torso.Position,
        Parent = hit.Parent.Torso
      })
      game:GetService("Debris"):AddItem(bp, 1)
    elseif Type == "Spike" then
      CreateBigIceSword(hit.Parent.Torso.CFrame)
      local bp = Create("BodyPosition")({
        P = 2000,
        D = 100,
        maxForce = Vector3.new(math.huge, math.huge, math.huge),
        position = hit.Parent.Torso.Position,
        Parent = hit.Parent.Torso
      })
      game:GetService("Debris"):AddItem(bp, 1)
    elseif Type == "Normal" then
      local BodPos = Create("BodyPosition")({
        P = 50000,
        D = 1000,
        maxForce = Vector3.new(math.huge, math.huge, math.huge),
        position = hit.Parent.Torso.Position,
        Parent = hit.Parent.Torso
      })
      local BodGy = Create("BodyGyro")({
        maxTorque = Vector3.new(400000, 400000, 400000) * math.huge,
        P = 20000,
        Parent = hit.Parent.Torso,
        cframe = hit.Parent.Torso.CFrame
      })
      hit.Parent.Torso.Anchored = true
      coroutine.resume(coroutine.create(function(Part)
        Part.Anchored = false
      end), hit.Parent.Torso)
      game:GetService("Debris"):AddItem(BodPos, 3)
      game:GetService("Debris"):AddItem(BodGy, 3)
    end
    local debounce = Create("BoolValue")({
      Name = "DebounceHit",
      Parent = hit.Parent,
      Value = true
    })
    game:GetService("Debris"):AddItem(debounce, Delay)
    c = Instance.new("ObjectValue")
    c.Name = "creator"
    c.Value = Player
    c.Parent = h
    game:GetService("Debris"):AddItem(c, 0.5)
  end
end
function ShowDamage(Pos, Text, Time, Color)
  local Rate = 0.1
  local Pos = Pos or Vector3.new(0, 0, 0)
  local Text = Text or ""
  local Time = Time or 2
  local Color = Color or Color3.new(1, 0, 1)
  local EffectPart = CreatePart(workspace, "SmoothPlastic", 0, 1, BrickColor.new(Color), "Effect", Vector3.new(0, 0, 0))
  EffectPart.Anchored = true
  local BillboardGui = Create("BillboardGui")({
    Size = UDim2.new(3, 0, 3, 0),
    Adornee = EffectPart,
    Parent = EffectPart
  })
  local TextLabel = Create("TextLabel")({
    BackgroundTransparency = 1,
    Size = UDim2.new(1, 0, 1, 0),
    Text = Text,
    TextColor3 = Color3.new(1,1,1),
    TextStrokeColor3 = Color3.new(0,0,0),
    TextStrokeTransparency = 0.25,
    TextScaled = true,
    Font = Enum.Font.Fantasy,
    TextSize = 24,
    Parent = BillboardGui
  })
  game.Debris:AddItem(EffectPart, Time + 0.1)
  EffectPart.Parent = game:GetService("Workspace")
  delay(0, function()
    local Frames = Time / Rate
    for Frame = 1, Frames do
      swait(Rate)
      local Percent = Frame / Frames
      TextLabel.Text = Text
      EffectPart.CFrame = CFrame.new(Pos) + Vector3.new(0, Percent*2, 0)
    end
    for Frame = 1, Frames do
      swait(Rate)
      local Percent = Frame / Frames
      TextLabel.Text = Text
    end
    for Frame = 1, Frames do
      swait(Rate)
      local Percent = Frame / Frames
      TextLabel.TextTransparency = Percent
      TextLabel.Text = Text
      TextLabel.TextStrokeTransparency = Percent
    end
    if EffectPart and EffectPart.Parent then
      EffectPart:Destroy()
    end
  end)
end
function MagniDamage(Part, magni, mindam, maxdam, knock, Type,Sound)
  for _, c in pairs(workspace:children()) do
    local hum = c:findFirstChildOfClass("Humanoid")
    if hum ~= nil then
      local head = c:findFirstChild("Torso")
      if head ~= nil then
        local targ = head.Position - Part.Position
        local mag = targ.magnitude
        if magni >= mag and c.Name ~= Player.Name then
          Damagefunc(head, head, mindam, maxdam, knock, Type, RootPart, 0.1, "rbxassetid://" ..Sound, 1)
        end
      end
      local head = c:findFirstChild("UpperTorso")
      if head ~= nil then
        local targ = head.Position - Part.Position
        local mag = targ.magnitude
        if magni >= mag and c.Name ~= Player.Name then
          Damagefunc(head, head, mindam, maxdam, knock, Type, RootPart, 0.1, "rbxassetid://" ..Sound, 1)
        end
      end
    end
  end
end


function rayCast(Pos, Dir, Max, Ignore)  -- Origin Position , Direction, MaxDistance , IgnoreDescendants
return game:service("Workspace"):FindPartOnRay(Ray.new(Pos, Dir.unit * (Max or 999.999)), Ignore) 
end 
----

function dmg(dude)
if dude.Name ~= Character then
local bgf = Instance.new("BodyGyro",dude.Head)
bgf.CFrame = bgf.CFrame * CFrame.fromEulerAnglesXYZ(math.rad(-90),0,0)
--[[local val = Instance.new("BoolValue",dude)
val.Name = "IsHit"]]--
local ds = coroutine.wrap(function()
for i, v in pairs(dude:GetChildren()) do
if v:IsA("Part") or v:IsA("MeshPart") then
v.Name = "DEMINISHED"
CFuncs["Sound"].Create("rbxassetid://763718160", v, 0.75, 1.1)
CFuncs["Sound"].Create("rbxassetid://782353443", v, 1, 1)
for i = 0, 1 do
sphere2(1,"Add",v.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),vt(1,1,1),-0.01,10,-0.01,BrickColor.new("Royal purple"),BrickColor.new("Royal purple").Color)
end
end
end
wait(0.5)
targetted = nil
CFuncs["Sound"].Create("rbxassetid://62339698", char, 0.25, 0.285)
coroutine.resume(coroutine.create(function()
for i, v in pairs(dude:GetChildren()) do
if v:IsA("Accessory") then

end
if v:IsA("Humanoid") then

end
if v:IsA("CharacterMesh") then

end
if v:IsA("Model") then

end
if v:IsA("Part") or v:IsA("MeshPart") then
for x, o in pairs(v:GetChildren()) do
if o:IsA("Decal") then
end
end
coroutine.resume(coroutine.create(function()
v.Material = "Neon"
v.CanCollide = false
v.Anchored = false
local bld = Instance.new("ParticleEmitter",v)
bld.LightEmission = 1
bld.Texture = "rbxassetid://363275192" ---284205403
bld.Color = ColorSequence.new(BrickColor.new("Royal purple").Color)
bld.Rate = 500
bld.Lifetime = NumberRange.new(1)
bld.Size = NumberSequence.new({NumberSequenceKeypoint.new(0,2,0),NumberSequenceKeypoint.new(0.8,2.25,0),NumberSequenceKeypoint.new(1,0,0)})
bld.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0,0.5,0),NumberSequenceKeypoint.new(0.8,0.75,0),NumberSequenceKeypoint.new(1,1,0)})
bld.Speed = NumberRange.new(2,5)
bld.VelocitySpread = 50000
bld.Rotation = NumberRange.new(-500,500)
bld.RotSpeed = NumberRange.new(-500,500)
local sbs = Instance.new("BodyPosition", v)
sbs.P = 3000
sbs.D = 1000
sbs.maxForce = Vector3.new(50000000000, 50000000000, 50000000000)
sbs.position = v.Position + Vector3.new(math.random(-2,2),10 + math.random(-2,2),math.random(-2,2))
v.Color = BrickColor.new("Royal purple").Color
coroutine.resume(coroutine.create(function()
for i = 0, 49 do
swait(1)
end
for i = 0, 4 do
slash(math.random(10,50)/10,3,true,"Round","Add","Out",v.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),vt(0.01,0.0025,0.01),math.random(10,100)/2500,BrickColor.new("White"))
end
block(1,"Add",v.CFrame,vt(0,0,0),0.1,0.1,0.1,BrickColor.new("Royal purple"),BrickColor.new("Royal purple").Color)
CFuncs["Sound"].Create("rbxassetid://782353117", v, 0.25, 1.2)
CFuncs["Sound"].Create("rbxassetid://1192402877", v, 0.5, 0.75)
bld.Speed = NumberRange.new(10,25)
bld.Drag = 5
bld.Acceleration = vt(0,2,0)
wait(0.5)
bld.Enabled = false
wait(4)
coroutine.resume(coroutine.create(function()
for i = 0, 99 do
swait()
end
end))
end))
end))
end
end
end))
end)
ds()
end
end

function sphere(bonuspeed,type,pos,scale,value,color)
local type = type
local rng = Instance.new("Part", char)
        rng.Anchored = true
        rng.BrickColor = color
        rng.CanCollide = false
        rng.FormFactor = 3
        rng.Name = "Ring"
        rng.Material = "Neon"
        rng.Size = Vector3.new(1, 1, 1)
        rng.Transparency = 0
        rng.TopSurface = 0
        rng.BottomSurface = 0
        rng.CFrame = pos
        local rngm = Instance.new("SpecialMesh", rng)
        rngm.MeshType = "Sphere"
rngm.Scale = scale
if rainbowmode == true then
rng.Color = Color3.new(r/255,g/255,b/255)
end
local scaler2 = 1
if type == "Add" then
scaler2 = 1*value
elseif type == "Divide" then
scaler2 = 1/value
end
coroutine.resume(coroutine.create(function()
for i = 0,10/bonuspeed,0.1 do
swait()
if rainbowmode == true then
rng.Color = Color3.new(r/255,g/255,b/255)
end
if type == "Add" then
scaler2 = scaler2 - 0.01*value/bonuspeed
elseif type == "Divide" then
scaler2 = scaler2 - 0.01/value*bonuspeed
end
if chaosmode == true then
rng.BrickColor = BrickColor.random()
end
rng.Transparency = rng.Transparency + 0.01*bonuspeed
rngm.Scale = rngm.Scale + Vector3.new(scaler2*bonuspeed, scaler2*bonuspeed, scaler2*bonuspeed)
end
rng:Destroy()
end))
end

function sphere2(bonuspeed,type,pos,scale,value,value2,value3,color,color3)
local type = type
local rng = Instance.new("Part", char)
        rng.Anchored = true
        rng.BrickColor = color
        rng.Color = color3
        rng.CanCollide = false
        rng.FormFactor = 3
        rng.Name = "Ring"
        rng.Material = "Neon"
        rng.Size = Vector3.new(1, 1, 1)
        rng.Transparency = 0
        rng.TopSurface = 0
        rng.BottomSurface = 0
        rng.CFrame = pos
        local rngm = Instance.new("SpecialMesh", rng)
        rngm.MeshType = "Sphere"
rngm.Scale = scale
local scaler2 = 1
local scaler2b = 1
local scaler2c = 1
if type == "Add" then
scaler2 = 1*value
scaler2b = 1*value2
scaler2c = 1*value3
elseif type == "Divide" then
scaler2 = 1/value
scaler2b = 1/value2
scaler2c = 1/value3
end
coroutine.resume(coroutine.create(function()
for i = 0,10/bonuspeed,0.1 do
swait()
if type == "Add" then
scaler2 = scaler2 - 0.01*value/bonuspeed
scaler2b = scaler2b - 0.01*value/bonuspeed
scaler2c = scaler2c - 0.01*value/bonuspeed
elseif type == "Divide" then
scaler2 = scaler2 - 0.01/value*bonuspeed
scaler2b = scaler2b - 0.01/value*bonuspeed
scaler2c = scaler2c - 0.01/value*bonuspeed
end
rng.Transparency = rng.Transparency + 0.01*bonuspeed
rngm.Scale = rngm.Scale + Vector3.new(scaler2*bonuspeed, scaler2b*bonuspeed, scaler2c*bonuspeed)
end
rng:Destroy()
end))
end

function block(bonuspeed,type,pos,scale,value,value2,value3,color,color3)
local type = type
local rng = Instance.new("Part", char)
        rng.Anchored = true
        rng.BrickColor = color
        rng.Color = color3
        rng.CanCollide = false
        rng.FormFactor = 3
        rng.Name = "Ring"
        rng.Material = "Neon"
        rng.Size = Vector3.new(1, 1, 1)
        rng.Transparency = 0
        rng.TopSurface = 0
        rng.BottomSurface = 0
        rng.CFrame = pos
        local rngm = Instance.new("SpecialMesh", rng)
        rngm.MeshType = "Brick"
rngm.Scale = scale
local scaler2 = 1
local scaler2b = 1
local scaler2c = 1
if type == "Add" then
scaler2 = 1*value
scaler2b = 1*value2
scaler2c = 1*value3
elseif type == "Divide" then
scaler2 = 1/value
scaler2b = 1/value2
scaler2c = 1/value3
end
coroutine.resume(coroutine.create(function()
for i = 0,10/bonuspeed,0.1 do
swait()
if type == "Add" then
scaler2 = scaler2 - 0.01*value/bonuspeed
scaler2b = scaler2b - 0.01*value/bonuspeed
scaler2c = scaler2c - 0.01*value/bonuspeed
elseif type == "Divide" then
scaler2 = scaler2 - 0.01/value*bonuspeed
scaler2b = scaler2b - 0.01/value*bonuspeed
scaler2c = scaler2c - 0.01/value*bonuspeed
end
rng.CFrame = rng.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360)))
rng.Transparency = rng.Transparency + 0.01*bonuspeed
rngm.Scale = rngm.Scale + Vector3.new(scaler2*bonuspeed, scaler2b*bonuspeed, scaler2c*bonuspeed)
end
rng:Destroy()
end))
end

function sphereMK(bonuspeed,FastSpeed,type,pos,x1,y1,z1,value,color,color3,outerpos)
local type = type
local rng = Instance.new("Part", char)
        rng.Anchored = true
        rng.BrickColor = color
        rng.Color = color3
        rng.CanCollide = false
        rng.FormFactor = 3
        rng.Name = "Ring"
        rng.Material = "Neon"
        rng.Size = Vector3.new(1, 1, 1)
        rng.Transparency = 0
        rng.TopSurface = 0
        rng.BottomSurface = 0
        rng.CFrame = pos
rng.CFrame = rng.CFrame + rng.CFrame.lookVector*outerpos
        local rngm = Instance.new("SpecialMesh", rng)
        rngm.MeshType = "Sphere"
rngm.Scale = vt(x1,y1,z1)
if rainbowmode == true then
rng.Color = Color3.new(r/255,g/255,b/255)
end
local scaler2 = 1
local speeder = FastSpeed
if type == "Add" then
scaler2 = 1*value
elseif type == "Divide" then
scaler2 = 1/value
end
coroutine.resume(coroutine.create(function()
for i = 0,10/bonuspeed,0.1 do
swait()
if rainbowmode == true then
rng.Color = Color3.new(r/255,g/255,b/255)
end
if type == "Add" then
scaler2 = scaler2 - 0.01*value/bonuspeed
elseif type == "Divide" then
scaler2 = scaler2 - 0.01/value*bonuspeed
end
if chaosmode == true then
rng.BrickColor = BrickColor.random()
end
speeder = speeder - 0.01*FastSpeed*bonuspeed
rng.CFrame = rng.CFrame + rng.CFrame.lookVector*speeder*bonuspeed
rng.Transparency = rng.Transparency + 0.01*bonuspeed
rngm.Scale = rngm.Scale + Vector3.new(scaler2*bonuspeed, scaler2*bonuspeed, 0)
end
rng:Destroy()
end))
end

function waveEff(bonuspeed,type,typeoftrans,pos,scale,value,value2,color)
local type = type
local rng = Instance.new("Part", char)
        rng.Anchored = true
        rng.BrickColor = color
        rng.CanCollide = false
        rng.FormFactor = 3
        rng.Name = "Ring"
        rng.Material = "Neon"
        rng.Size = Vector3.new(1, 1, 1)
        rng.Transparency = 0
if typeoftrans == "In" then
rng.Transparency = 1
end
        rng.TopSurface = 0
        rng.BottomSurface = 0
        rng.CFrame = pos
        local rngm = Instance.new("SpecialMesh", rng)
        rngm.MeshType = "FileMesh"
rngm.MeshId = "rbxassetid://20329976"
rngm.Scale = scale
local scaler2 = 1
local scaler2b = 1
if type == "Add" then
scaler2 = 1*value
scaler2b = 1*value2
elseif type == "Divide" then
scaler2 = 1/value
scaler2b = 1/value2
end
local randomrot = math.random(1,2)
coroutine.resume(coroutine.create(function()
for i = 0,10/bonuspeed,0.1 do
swait()
if type == "Add" then
scaler2 = scaler2 - 0.01*value/bonuspeed
scaler2b = scaler2b - 0.01*value/bonuspeed
elseif type == "Divide" then
scaler2 = scaler2 - 0.01/value*bonuspeed
scaler2b = scaler2b - 0.01/value*bonuspeed
end
if randomrot == 1 then
rng.CFrame = rng.CFrame*CFrame.Angles(0,math.rad(5*bonuspeed/2),0)
elseif randomrot == 2 then
rng.CFrame = rng.CFrame*CFrame.Angles(0,math.rad(-5*bonuspeed/2),0)
end
if typeoftrans == "Out" then
rng.Transparency = rng.Transparency + 0.01*bonuspeed
elseif typeoftrans == "In" then
rng.Transparency = rng.Transparency - 0.01*bonuspeed
end
rngm.Scale = rngm.Scale + Vector3.new(scaler2*bonuspeed, scaler2b*bonuspeed, scaler2*bonuspeed)
end
rng:Destroy()
end))
end

function slash(bonuspeed,rotspeed,rotatingop,typeofshape,type,typeoftrans,pos,scale,value,color)
local type = type
local rotenable = rotatingop
local rng = Instance.new("Part", char)
        rng.Anchored = true
        rng.BrickColor = color
        rng.CanCollide = false
        rng.FormFactor = 3
        rng.Name = "Ring"
        rng.Material = "Neon"
        rng.Size = Vector3.new(1, 1, 1)
        rng.Transparency = 0
if typeoftrans == "In" then
rng.Transparency = 1
end
        rng.TopSurface = 0
        rng.BottomSurface = 0
        rng.CFrame = pos
        local rngm = Instance.new("SpecialMesh", rng)
        rngm.MeshType = "FileMesh"
if typeofshape == "Normal" then
rngm.MeshId = "rbxassetid://662586858"
elseif typeofshape == "Round" then
rngm.MeshId = "rbxassetid://662585058"
end
rngm.Scale = scale
local scaler2 = 1/10
if type == "Add" then
scaler2 = 1*value/10
elseif type == "Divide" then
scaler2 = 1/value/10
end
local randomrot = math.random(1,2)
coroutine.resume(coroutine.create(function()
for i = 0,10/bonuspeed,0.1 do
swait()
if type == "Add" then
scaler2 = scaler2 - 0.01*value/bonuspeed/10
elseif type == "Divide" then
scaler2 = scaler2 - 0.01/value*bonuspeed/10
end
if rotenable == true then
if randomrot == 1 then
rng.CFrame = rng.CFrame*CFrame.Angles(0,math.rad(rotspeed*bonuspeed/2),0)
elseif randomrot == 2 then
rng.CFrame = rng.CFrame*CFrame.Angles(0,math.rad(-rotspeed*bonuspeed/2),0)
end
end
if typeoftrans == "Out" then
rng.Transparency = rng.Transparency + 0.01*bonuspeed
elseif typeoftrans == "In" then
rng.Transparency = rng.Transparency - 0.01*bonuspeed
end
rngm.Scale = rngm.Scale + Vector3.new(scaler2*bonuspeed/10, 0, scaler2*bonuspeed/10)
end
rng:Destroy()
end))
end

rarmor.Attachment.Name = "Attachment2"
function FindNearestTorso(Position, Distance, SinglePlayer)
	if SinglePlayer then
		return (SinglePlayer.Torso.CFrame.p - Position).magnitude < Distance
	end
	local List = {}
	for i, v in pairs(workspace:GetChildren()) do
		if v:IsA("Model") then
			if v:findFirstChild("Torso") or v:findFirstChild("UpperTorso") then
				if v ~= Character then
					if (v.Head.Position - Position).magnitude <= Distance then
						table.insert(List, v)
					end 
				end 
			end 
		end 
	end
	return List
end


local dashing = false
local floatmode = false
local OWS = hum.WalkSpeed
local equipped = false
Instance.new("ForceField",char).Visible = false
Humanoid.Animator.Parent = nil
------------------
function equip()
	attack = true
	equipped = true
	hum.WalkSpeed = 0
tl1.Enabled = true
for i = 0, 9 do
end
CFuncs["Sound"].Create("rbxassetid://1368637781", rarmor, 2.5, 1.25)
CFuncs["Sound"].Create("rbxassetid://200633077", rarmor, 1, 1)
CFuncs["Sound"].Create("rbxassetid://169380495", rarmor, 0.5, 1.1)
	for i = 0, 2, 0.1 do
		swait()
hum.CameraOffset = vt(math.random(-5,5)/50,math.random(-5,5)/50,math.random(-5,5)/50)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(10),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0)*angles(math.rad(0),math.rad(0),math.rad(-10)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2),math.rad(0),math.rad(-20)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(-20),math.rad(-30),math.rad(130)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(-13),math.rad(10),math.rad(-10)),.3)
end
hum.CameraOffset = vt(0,0,0)
weaponweld.Part0 = rarm
for i = 0, 2, 0.1 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-40),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(1),math.rad(5)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.1,0.1,0)*angles(math.rad(0),math.rad(0),math.rad(40)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2),math.rad(0),math.rad(-40)),.3)
RW.C0=clerp(RW.C0,cf(1.25,0.5,-0.65)*angles(math.rad(100),math.rad(0),math.rad(-23)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(110),math.rad(0),math.rad(-85)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(0),math.rad(0)),.3)
end
local hitb = CreateParta(m,1,1,"SmoothPlastic",BrickColor.Random())
hitb.Anchored = true
hitb.CFrame = root.CFrame + root.CFrame.lookVector*4
MagniDamage(hitb, 4, 40,73, 0, "Normal",153092213)
CFuncs["Sound"].Create("rbxassetid://200633196", rarmor, 1, 1.05)
CFuncs["Sound"].Create("rbxassetid://200633108", rarmor, 1.5, 1.025)
CFuncs["Sound"].Create("rbxassetid://234365549", rarmor, 1, 1)
for i = 0, 2, 0.1 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-20)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(50),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(-0.1,-0.25,0)*angles(math.rad(10),math.rad(0),math.rad(-50)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2),math.rad(0),math.rad(50)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(80),math.rad(0),math.rad(70)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(100),math.rad(0),math.rad(-50)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
end
hitb:Destroy()
hum.WalkSpeed = Speed
OWS = hum.WalkSpeed
	attack = false
end

function unequip()
	attack = true
	equipped = false
	hum.WalkSpeed = 0
hum.WalkSpeed = 16
OWS = hum.WalkSpeed
tl1.Enabled = false
CFuncs["Sound"].Create("rbxassetid://200633029", rarmor, 1, 1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.5)
weaponweld.Part0 = tors
	attack = false
end

------------------
function attackone()
attack = true
hum.WalkSpeed = Speed
for i = 0, 2, 0.1 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-40),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(1),math.rad(5)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.1,0.1,0)*angles(math.rad(0),math.rad(0),math.rad(40)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2),math.rad(0),math.rad(-40)),.3)
RW.C0=clerp(RW.C0,cf(1.25,0.5,-0.65)*angles(math.rad(100),math.rad(0),math.rad(-23)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(110),math.rad(0),math.rad(-85)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(0),math.rad(0)),.3)
end
local hitb = CreateParta(m,1,1,"SmoothPlastic",BrickColor.Random())
hitb.Anchored = true
hitb.CFrame = root.CFrame + root.CFrame.lookVector*4
MagniDamage(hitb, 4, 24,30, 0, "Normal",153092213)
CFuncs["Sound"].Create("rbxassetid://200633196", rarmor, 1, 1.05)
CFuncs["Sound"].Create("rbxassetid://200633108", rarmor, 1.5, 1.025)
CFuncs["Sound"].Create("rbxassetid://234365549", rarmor, 1, 1)
for i = 0, 1, 0.1 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-20)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(50),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(-0.1,-0.25,0)*angles(math.rad(10),math.rad(0),math.rad(-50)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2),math.rad(0),math.rad(50)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(80),math.rad(0),math.rad(70)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(100),math.rad(0),math.rad(-50)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
end
hitb:Destroy()
attack = false
hum.WalkSpeed = Speed
end
function attacktwo()
attack = true
hum.WalkSpeed = Speed
for i = 0, 1, 0.1 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(20),math.rad(5)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(-0.1,0.1,0)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2),math.rad(0),math.rad(40)),.3)
RW.C0=clerp(RW.C0,cf(1.25,0.5,-0.65)*angles(math.rad(100),math.rad(0),math.rad(-23)),.3)
LW.C0=clerp(LW.C0,cf(-0.5,0.5,-0.25)*angles(math.rad(90),math.rad(0),math.rad(40)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(180),math.rad(0)),.3)
end
local hitb = CreateParta(m,1,1,"SmoothPlastic",BrickColor.Random())
hitb.Anchored = true
hitb.CFrame = root.CFrame + root.CFrame.lookVector*4
MagniDamage(hitb, 4, 24,30, 0, "Normal",153092213)
CFuncs["Sound"].Create("rbxassetid://200633281", rarmor, 1, 1.05)
CFuncs["Sound"].Create("rbxassetid://161006195", rarmor, 1.5, 1.025)
for i = 0, 1, 0.1 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-30),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(20)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.2,-0.25,0)*angles(math.rad(10),math.rad(0),math.rad(90)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2),math.rad(0),math.rad(-90)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(80),math.rad(0),math.rad(20)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(100),math.rad(0),math.rad(-50)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(180),math.rad(70)),.3)
end
attack = false
hum.WalkSpeed = Speed
end
function attackthree()
attack = true
hum.WalkSpeed = Speed
for i = 0, 1, 0.1 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-30),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(5)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(-0.1,0.1,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0)*angles(math.rad(-30),math.rad(0),math.rad(53)),.3)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0)*angles(math.rad(10),math.rad(0),math.rad(-10)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(90),math.rad(-20)),.3)
end
for x = 0, 2 do
CFuncs["Sound"].Create("rbxassetid://200633108", rarmor, 1, 1.05)
CFuncs["Sound"].Create("rbxassetid://234365573", rarmor, 1.5, 1.025)
local hitb = CreateParta(m,1,1,"SmoothPlastic",BrickColor.Random())
hitb.Anchored = true
hitb.CFrame = root.CFrame + root.CFrame.lookVector*4
MagniDamage(hitb, 4, 12,15, 0, "Normal",153092213)
for i = 0, 1, 0.6 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(40),math.rad(20)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.2,-0.25,0)*angles(math.rad(-2),math.rad(0),math.rad(80)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-80)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-20)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(0),math.rad(30),math.rad(90)),.3)
end
for i = 0, 1, 0.6 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(40),math.rad(20)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.2,-0.25,0)*angles(math.rad(-2),math.rad(0),math.rad(80)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-80)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-20)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(0),math.rad(0),math.rad(180)),.3)
end
for i = 0, 1, 0.6 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(40),math.rad(20)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.2,-0.25,0)*angles(math.rad(-2),math.rad(0),math.rad(80)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-80)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-20)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(0),math.rad(-30),math.rad(270)),.3)
end
for i = 0, 1, 0.6 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(40),math.rad(20)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.2,-0.25,0)*angles(math.rad(-2),math.rad(0),math.rad(80)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-80)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-20)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(0),math.rad(0),math.rad(0)),.3)
end
end
attack = false
hum.WalkSpeed = Speed
end
------------------
function spinnyblade()
attack = true
hum.WalkSpeed = 16
hum.JumpPower = 0
CFuncs["Sound"].Create("rbxassetid://1368583274", root, 4.5, 1)
local bgui = Instance.new("BillboardGui",root)
bgui.Size = UDim2.new(25, 0, 25, 0)
local imgc = Instance.new("ImageLabel",bgui)
imgc.BackgroundTransparency = 1
imgc.ImageTransparency = 1
imgc.Size = UDim2.new(1,0,1,0)
imgc.Image = "rbxassetid://997291547"
imgc.ImageColor3 = Color3.new(0,0.5,1)
local imgc2 = imgc:Clone()
imgc2.Parent = bgui
imgc2.Position = UDim2.new(-0.5,0,-0.5,0)
imgc2.Size = UDim2.new(2,0,2,0)
imgc2.ImageColor3 = Color3.new(0.5,0,1)
for i = 0, 1, 0.1 do
		swait()
bgui.Size = bgui.Size - UDim2.new(0.25, 0, 0.25, 0)
hum.CameraOffset = vt(math.random(-10,10)/50,math.random(-10,10)/50,math.random(-10,10)/50)
	RH.C0=clerp(RH.C0,cf(1,-0.5,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-40),math.rad(10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(1),math.rad(20)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.1,0.2,-0.3)*angles(math.rad(10),math.rad(0),math.rad(50)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(5),math.rad(0),math.rad(-50)),.3)
RW.C0=clerp(RW.C0,cf(1.25,0.5,-0.65)*angles(math.rad(100),math.rad(0),math.rad(-23)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(110),math.rad(0),math.rad(-85)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(0),math.rad(0)),.3)
end
imgc.ImageTransparency = 1
hum.CameraOffset = vt(0,0,0)
for i = 0, 9 do
end
CFuncs["Sound"].Create("rbxassetid://430315987", root, 1.5, 1)
CFuncs["Sound"].Create("rbxassetid://1295446488", root, 3, 1)
for x = 0, 20 do
CFuncs["Sound"].Create("rbxassetid://200633281", rarmor, 1, 1.05)
CFuncs["Sound"].Create("rbxassetid://161006195", rarmor, 1.5, 1.025)
MagniDamage(tors, 10, 60,85, 0, "Normal",153092213)
CFuncs["Sound"].Create("rbxassetid://200632992", rarmor, 1.25, 1)
for i = 0, 1, 0.6 do
		swait()
root.CFrame = root.CFrame + root.CFrame.lookVector*2
root.Velocity = vt(0,0,0)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0.5)*angles(math.rad(0),math.rad(0),math.rad(90)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(90)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
end
CFuncs["Sound"].Create("rbxassetid://200632992", rarmor, 1.25, 1)
MagniDamage(tors, 10, 60,85, 0, "Normal",153092213)
for i = 0, 1, 0.6 do
		swait()
root.CFrame = root.CFrame + root.CFrame.lookVector*3
root.Velocity = vt(0,0,0)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0.5)*angles(math.rad(90),math.rad(0),math.rad(90)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(90)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
end
CFuncs["Sound"].Create("rbxassetid://200632992", rarmor, 1.25, 1)
MagniDamage(tors, 10, 60,85, 0, "Normal",153092213)
for i = 0, 1, 0.6 do
		swait()
root.CFrame = root.CFrame + root.CFrame.lookVector*3
root.Velocity = vt(0,0,0)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0.5)*angles(math.rad(180),math.rad(0),math.rad(90)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(90)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
end
CFuncs["Sound"].Create("rbxassetid://200632992", rarmor, 1.25, 1)
MagniDamage(tors, 10, 60,85, 0, "Normal",153092213)
for i = 0, 1, 0.6 do
		swait()
root.CFrame = root.CFrame + root.CFrame.lookVector*3
root.Velocity = vt(0,0,0)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0.5)*angles(math.rad(270),math.rad(0),math.rad(90)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(90)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
end
end
hum.WalkSpeed = 0
for i = 0, 5, 0.1 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-20)),.2)
LH.C0=clerp(LH.C0,cf(-1,-0.6,-0.5)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(20),math.rad(-12)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.1,0.2,-0.35)*angles(math.rad(10),math.rad(0),math.rad(-40)),.2)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(5),math.rad(0),math.rad(40)),.2)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0)*angles(math.rad(90),math.rad(0),math.rad(110)),.2)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0)*angles(math.rad(45),math.rad(0),math.rad(-20)),.2)
weaponweld.C1=clerp(weaponweld.C1,cf(2,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.2)
end
bgui:Destroy()
attack = false
hum.WalkSpeed = Speed
hum.JumpPower = 50
end

function darkspin()
attack = true
hum.WalkSpeed = 16
hum.JumpPower = 0
CFuncs["Sound"].Create("rbxassetid://1368583274", root, 4.5, 1)
local bgui = Instance.new("BillboardGui",root)
bgui.Size = UDim2.new(25, 0, 25, 0)
local imgc = Instance.new("ImageLabel",bgui)
imgc.BackgroundTransparency = 1
imgc.ImageTransparency = 1
imgc.Size = UDim2.new(1,0,1,0)
imgc.Image = "rbxassetid://997291547"
imgc.ImageColor3 = Color3.new(0,0.5,1)
local imgc2 = imgc:Clone()
imgc2.Parent = bgui
imgc2.Position = UDim2.new(-0.5,0,-0.5,0)
imgc2.Size = UDim2.new(2,0,2,0)
imgc2.ImageColor3 = Color3.new(0.5,0,1)
for i = 0, 1, 0.1 do
		swait()
bgui.Size = bgui.Size - UDim2.new(0.25, 0, 0.25, 0)
hum.CameraOffset = vt(math.random(-10,10)/50,math.random(-10,10)/50,math.random(-10,10)/50)
	RH.C0=clerp(RH.C0,cf(1,-0.5,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-40),math.rad(10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(1),math.rad(20)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.1,0.2,-0.3)*angles(math.rad(10),math.rad(0),math.rad(50)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(5),math.rad(0),math.rad(-50)),.3)
RW.C0=clerp(RW.C0,cf(1.25,0.5,-0.65)*angles(math.rad(100),math.rad(0),math.rad(-23)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(110),math.rad(0),math.rad(-85)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(0),math.rad(0)),.3)
end
imgc.ImageTransparency = 1
hum.CameraOffset = vt(0,0,0)
for i = 0, 9 do
end
CFuncs["Sound"].Create("rbxassetid://430315987", root, 1.5, 1)
CFuncs["Sound"].Create("rbxassetid://1295446488", root, 3, 1)
for x = 0, 20 do
CFuncs["Sound"].Create("rbxassetid://200633281", rarmor, 1, 1.05)
CFuncs["Sound"].Create("rbxassetid://161006195", rarmor, 1.5, 1.025)
MagniDamage(tors, 10, 60,85, 0, "Normal",153092213)
CFuncs["Sound"].Create("rbxassetid://200632992", rarmor, 1.25, 1)
for i = 0, 1, 0.6 do
		swait()
root.CFrame = root.CFrame + root.CFrame.lookVector*6
root.Velocity = vt(0,0,0)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0.5)*angles(math.rad(0),math.rad(0),math.rad(90)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(90)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
end
CFuncs["Sound"].Create("rbxassetid://200632992", rarmor, 1.25, 1)
MagniDamage(tors, 10, 60,85, 0, "Normal",153092213)
for i = 0, 1, 0.6 do
		swait()
root.CFrame = root.CFrame + root.CFrame.lookVector*4
root.Velocity = vt(0,0,0)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0.5)*angles(math.rad(90),math.rad(0),math.rad(90)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(90)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
end
CFuncs["Sound"].Create("rbxassetid://200632992", rarmor, 1.25, 1)
MagniDamage(tors, 10, 60,85, 0, "Normal",153092213)
for i = 0, 1, 0.6 do
		swait()
root.CFrame = root.CFrame + root.CFrame.lookVector*5
root.Velocity = vt(0,0,0)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0.5)*angles(math.rad(180),math.rad(0),math.rad(90)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(90)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
end
CFuncs["Sound"].Create("rbxassetid://200632992", rarmor, 1.25, 1)
MagniDamage(tors, 10, 60,85, 0, "Normal",153092213)
for i = 0, 1, 0.6 do
		swait()
root.CFrame = root.CFrame + root.CFrame.lookVector*4
root.Velocity = vt(0,0,0)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0.5)*angles(math.rad(270),math.rad(0),math.rad(90)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(90)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.3)
end
end
hum.WalkSpeed = 0
for i = 0, 5, 0.1 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-20)),.2)
LH.C0=clerp(LH.C0,cf(-1,-0.6,-0.5)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(20),math.rad(-12)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0.1,0.2,-0.35)*angles(math.rad(10),math.rad(0),math.rad(-40)),.2)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(5),math.rad(0),math.rad(40)),.2)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0)*angles(math.rad(90),math.rad(0),math.rad(110)),.2)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0)*angles(math.rad(45),math.rad(0),math.rad(-20)),.2)
weaponweld.C1=clerp(weaponweld.C1,cf(2,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.2)
end
bgui:Destroy()
attack = false
hum.WalkSpeed = Speed
hum.JumpPower = 50
end

function eightbitmegablade()
attack = true
hum.WalkSpeed = 0
hum.JumpPower = 0
CFuncs["Sound"].Create("rbxassetid://1368583274", larm, 4.5, 1.2)
local OverCut = false
cam.CameraSubject = Humanoid
cam.CameraType = "Scriptable"
coroutine.resume(coroutine.create(function()
while true do
swait()
if OverCut == false then
cam.CFrame = lerp(cam.CFrame, root.CFrame * cf(1, 1.5, -6) * ceuler(math.rad(10), math.rad(170), math.rad(-20)), 0.1)
else
break
end
end
end))
for i = 0, 4, 0.1 do
swait()
sphere2(5,"Add",larm.CFrame*CFrame.new(0,-1.5,0)*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),vt(1,1,1),-0.01,0.1,-0.01,BrickColor.new("Toothpaste"),BrickColor.new("Toothpaste").Color)
slash(math.random(20,40)/10,5,true,"Round","Add","Out",larm.CFrame*CFrame.new(0,-1.5,0)*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),vt(0.025,0.001,0.025),-0.025,BrickColor.new("White"))
RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-6),math.rad(0),math.rad(-6)),.3)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(3)),.3)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0)*angles(math.rad(0),math.rad(0),math.rad(-50)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-15),math.rad(5),math.rad(50)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(-13),math.rad(-40),math.rad(20)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(170),math.rad(10),math.rad(0)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(130),math.rad(0)),.3)
end
OverCut = true
local orb = Instance.new("Part", char)
orb.Anchored = true
orb.BrickColor = BrickColor.new("Toothpaste")
orb.CanCollide = false
orb.FormFactor = 3
orb.Name = "Ring"
orb.Material = "Neon"
orb.Size = Vector3.new(1, 1, 1)
orb.Transparency = 0.5
orb.TopSurface = 0
orb.BottomSurface = 0
local orbm = Instance.new("SpecialMesh", orb)
orbm.MeshType = "FileMesh"
orbm.MeshId = "rbxassetid://361629844"
orbm.Scale = vt(30,60,60)
orb.CFrame = root.CFrame*CFrame.new(0,50,0)
for i = 0, 24 do
end
CFuncs["Sound"].Create("rbxassetid://1368637781", orb, 7.5, 1)
local a = Instance.new("Part",workspace)
a.Name = "Direction"	
a.Anchored = true
a.Transparency = 1
a.CanCollide = false
local ray = Ray.new(
orb.CFrame.p,                           -- origin
(mouse.Hit.p - orb.CFrame.p).unit * 500 -- direction
) 
local ignore = orb
local hit, position, normal = workspace:FindPartOnRay(ray, ignore)
a.BottomSurface = 10
a.TopSurface = 10
local distance = (orb.CFrame.p - position).magnitude
a.Size = Vector3.new(0.1, 0.1, 0.1)
a.CFrame = CFrame.new(orb.CFrame.p, position) * CFrame.new(0, 0, 0)
orb.CFrame = a.CFrame
for i = 0, 7, 0.1 do
swait()
ray = Ray.new(
orb.CFrame.p,                           -- origin
(mouse.Hit.p - orb.CFrame.p).unit * 500 -- direction
) 
hit, position, normal = workspace:FindPartOnRay(ray, ignore)
distance = (orb.CFrame.p - position).magnitude
a.CFrame = CFrame.new(orb.CFrame.p, position) * CFrame.new(0, 0, 0)
orb.CFrame = a.CFrame
cam.CFrame = lerp(cam.CFrame, root.CFrame * cf(20, 65, 55) * ceuler(math.rad(-20), math.rad(0), math.rad(10)), 0.2)
RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-6),math.rad(0),math.rad(-6)),.3)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(40),math.rad(3)),.3)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0)*angles(math.rad(0),math.rad(0),math.rad(-90)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(5),math.rad(0),math.rad(90)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(-13),math.rad(-20),math.rad(20)),.3)
LW.C0=clerp(LW.C0,cf(-1.25,0.5,-0.5)*angles(math.rad(100),math.rad(0),math.rad(60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(130),math.rad(0)),.3)
end
cam.CameraType = "Custom"
orb.Anchored = false
a:Destroy()
local bv = Instance.new("BodyVelocity")
bv.maxForce = Vector3.new(1e9, 1e9, 1e9)
bv.velocity = orb.CFrame.lookVector*250
bv.Parent = orb
local hitted = false
CFuncs["Sound"].Create("rbxassetid://466493476", orb, 7.5, 0.7)
waveEff(2,"Add","Out",orb.CFrame*CFrame.Angles(math.rad(90),math.rad(math.random(-360,360)),0),vt(5,1,5),0.5,0.1,BrickColor.new("Cyan"))
coroutine.resume(coroutine.create(function()
while true do
swait(2)
if hitted == false and orb.Parent ~= nil then
elseif hitted == true and orb.Parent == nil then
break
end
end
end))
orb.Touched:connect(function(hit) 
if hitted == false and hit.Parent ~= char then
hitted = true
MagniDamage(orb, 30, 72,95, 0, "Normal",153092213)
CFuncs["Sound"].Create("rbxassetid://763717897", orb, 10, 1)
CFuncs["Sound"].Create("rbxassetid://1295446488", orb, 9, 0.75)
for i = 0, 24 do
end
orb.Anchored = true
orb.Transparency = 1
coroutine.resume(coroutine.create(function()
for i = 0, 4, 0.1 do
swait()
hum.CameraOffset = vt(math.random(-10,10)/25,math.random(-10,10)/25,math.random(-10,10)/25)
end
hum.CameraOffset = vt(0,0,0)
end))
wait(10)
orb:Destroy()
end
end)
game:GetService("Debris"):AddItem(orb, 10)
for i = 0, 2, 0.1 do
swait()
RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-6),math.rad(0),math.rad(-6)),.3)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(3)),.3)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.4,0)*angles(math.rad(0),math.rad(0),math.rad(-70)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(5),math.rad(0),math.rad(70)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(-13),math.rad(-40),math.rad(20)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-80)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(130),math.rad(0)),.3)
end
attack = false
hum.WalkSpeed = Speed
hum.JumpPower = 50
end

function bladespinagain()
attack = true
hum.WalkSpeed = 40
hum.JumpPower = 0
CFuncs["Sound"].Create("rbxassetid://1368598393", rarmor, 2, 1)
CFuncs["Sound"].Create("rbxassetid://1368583274", rarmor, 2.5, 1)
for x = 0, 1 do
CFuncs["Sound"].Create("rbxassetid://200633108", rarmor, 2, 1.05)
CFuncs["Sound"].Create("rbxassetid://234365573", rarmor, 2.5, 1.025)
for i = 0, 1, 0.6 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.25,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(0),math.rad(0)),.3)
end
for i = 0, 1, 0.6 do
		swait()
hum.CameraOffset = vt(math.random(-10,10)/100,math.random(-10,10)/100,math.random(-10,10)/100)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.25,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(90),math.rad(0)),.3)
end
for i = 0, 1, 0.6 do
		swait()
hum.CameraOffset = vt(math.random(-10,10)/100,math.random(-10,10)/100,math.random(-10,10)/100)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.25,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(180),math.rad(0)),.3)
end
for i = 0, 1, 0.6 do
		swait()
hum.CameraOffset = vt(math.random(-10,10)/100,math.random(-10,10)/100,math.random(-10,10)/100)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.25,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(270),math.rad(0)),.3)
end
end
local hitb = CreateParta(m,1,1,"SmoothPlastic",BrickColor.Random())
hitb.Anchored = true
hitb.CFrame = root.CFrame + root.CFrame.lookVector*8
hitb.CFrame = hitb.CFrame*CFrame.new(0,1,0)
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
for i = 0, 24 do
end
CFuncs["Sound"].Create("rbxassetid://313205954", root, 4,1)
CFuncs["Sound"].Create("rbxassetid://1368637781", rarmor, 4,1)
CFuncs["Sound"].Create("rbxassetid://763718160", rarmor, 5, 1.1)
CFuncs["Sound"].Create("rbxassetid://782353443", rarmor, 6, 1)
--CFuncs["Sound"].Create("rbxassetid://1548538202", rarmor, 4,1)
for i = 0, 2, 0.1 do
		swait()
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
hum.CameraOffset = vt(math.random(-10,10)/25,math.random(-10,10)/25,math.random(-10,10)/25)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-20),math.rad(-10)),.9)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.9)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.5,0)*angles(math.rad(0),math.rad(0),math.rad(80)),.9)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-80)),.9)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(70)),.9)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-60)),.9)
weaponweld.C1=clerp(weaponweld.C1,cf(2,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.9)
end
hum.CameraOffset = vt(0,0,0)
hitb:Destroy()
attack = false
hum.WalkSpeed = Speed
hum.JumpPower = 50
end

function darkbruh()
attack = true
hum.WalkSpeed = hum.WalkSpeed
hum.JumpPower = 0
CFuncs["Sound"].Create("rbxassetid://1368598393", rarmor, 2, 1)
CFuncs["Sound"].Create("rbxassetid://1368583274", rarmor, 2.5, 1)
for x = 0, 1 do
CFuncs["Sound"].Create("rbxassetid://200633108", rarmor, 2, 1.05)
CFuncs["Sound"].Create("rbxassetid://234365573", rarmor, 2.5, 1.025)
for i = 0, 1, 0.6 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.5,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(0),math.rad(0)),.3)
end
for i = 0, 1, 0.6 do
		swait()
hum.CameraOffset = vt(math.random(-10,10)/100,math.random(-10,10)/100,math.random(-10,10)/100)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.25,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(90),math.rad(0)),.3)
end
for i = 0, 1, 0.6 do
		swait()
hum.CameraOffset = vt(math.random(-10,10)/100,math.random(-10,10)/100,math.random(-10,10)/100)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.25,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(180),math.rad(0)),.3)
end
end
local hitb = CreateParta(m,1,1,"SmoothPlastic",BrickColor.Random())
hitb.Anchored = true
hitb.CFrame = root.CFrame + root.CFrame.lookVector*16
hitb.CFrame = hitb.CFrame*CFrame.new(0,1,0)
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
for i = 0, 24 do
end
CFuncs["Sound"].Create("rbxassetid://313205954", root, 4,1)
CFuncs["Sound"].Create("rbxassetid://1368637781", rarmor, 4,1)
CFuncs["Sound"].Create("rbxassetid://763718160", rarmor, 5, 1.1)
CFuncs["Sound"].Create("rbxassetid://782353443", rarmor, 6, 1)
--CFuncs["Sound"].Create("rbxassetid://1548538202", rarmor, 4,1)
for i = 0, 2, 0.1 do
		swait()
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
hum.CameraOffset = vt(math.random(-10,10)/25,math.random(-10,10)/25,math.random(-10,10)/25)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-20),math.rad(-10)),.9)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.9)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.5,0)*angles(math.rad(0),math.rad(0),math.rad(80)),.9)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-80)),.9)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(70)),.9)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-60)),.9)
weaponweld.C1=clerp(weaponweld.C1,cf(2,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.9)
end
hum.CameraOffset = vt(0,0,0)
hitb:Destroy()
attack = false
hum.WalkSpeed = Speed
hum.JumpPower = 50
end

function SCYTHEslash2()
attack = true
hum.WalkSpeed = 90
hum.JumpPower = 0
CFuncs["Sound"].Create("rbxassetid://1368598393", rarmor, 2, 1)
CFuncs["Sound"].Create("rbxassetid://1368583274", rarmor, 2.5, 1)
for x = 0, 1 do
CFuncs["Sound"].Create("rbxassetid://200633108", rarmor, 2, 1.05)
CFuncs["Sound"].Create("rbxassetid://234365573", rarmor, 2.5, 1.025)
for i = 0, 1, 0.6 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.25,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(65),math.rad(0)),.3)
end
end
wait(0.05)
local hitb = CreateParta(m,1,1,"SmoothPlastic",BrickColor.Random())
hitb.Anchored = true
hitb.CFrame = root.CFrame + root.CFrame.lookVector*8
hitb.CFrame = hitb.CFrame*CFrame.new(0,1,0)
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
for i = 0, 24 do
end
CFuncs["Sound"].Create("rbxassetid://313205954", root, 4,1)
CFuncs["Sound"].Create("rbxassetid://1368637781", rarmor, 4,1)
CFuncs["Sound"].Create("rbxassetid://763718160", rarmor, 5, 1.1)
CFuncs["Sound"].Create("rbxassetid://782353443", rarmor, 6, 1)
--CFuncs["Sound"].Create("rbxassetid://1548538202", rarmor, 4,1)
for i = 0, 2, 0.1 do
		swait()
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
hum.CameraOffset = vt(math.random(-10,10)/25,math.random(-10,10)/25,math.random(-10,10)/25)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-20),math.rad(-10)),.9)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.9)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.5,0)*angles(math.rad(0),math.rad(0),math.rad(80)),.9)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-80)),.9)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(70)),.9)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-60)),.9)
weaponweld.C1=clerp(weaponweld.C1,cf(2,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.9)
end
hum.CameraOffset = vt(0,0,0)
hitb:Destroy()
attack = false
hum.WalkSpeed = Speed
hum.JumpPower = 50
end

function axeslash()
attack = true
hum.WalkSpeed = 100
hum.JumpPower = 0
CFuncs["Sound"].Create("rbxassetid://1368598393", rarmor, 2, 1)
CFuncs["Sound"].Create("rbxassetid://1368583274", rarmor, 2.5, 1)
for x = 0, 1 do
CFuncs["Sound"].Create("rbxassetid://200633108", rarmor, 2, 1.05)
CFuncs["Sound"].Create("rbxassetid://234365573", rarmor, 2.5, 1.025)
for i = 0, 1, 0.6 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.25,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(65),math.rad(0)),.3)
end
end
local hitb = CreateParta(m,1,1,"SmoothPlastic",BrickColor.Random())
hitb.Anchored = true
hitb.CFrame = root.CFrame + root.CFrame.lookVector*8
hitb.CFrame = hitb.CFrame*CFrame.new(0,1,0)
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
for i = 0, 24 do
end
CFuncs["Sound"].Create("rbxassetid://313205954", root, 4,1)
CFuncs["Sound"].Create("rbxassetid://1368637781", rarmor, 4,1)
CFuncs["Sound"].Create("rbxassetid://763718160", rarmor, 5, 1.1)
CFuncs["Sound"].Create("rbxassetid://782353443", rarmor, 6, 1)
--CFuncs["Sound"].Create("rbxassetid://1548538202", rarmor, 4,1)
for i = 0, 2, 0.1 do
		swait()
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
hum.CameraOffset = vt(math.random(-10,10)/25,math.random(-10,10)/25,math.random(-10,10)/25)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-20),math.rad(-10)),.9)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.9)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.5,0)*angles(math.rad(0),math.rad(0),math.rad(80)),.9)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-80)),.9)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(70)),.9)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-60)),.9)
weaponweld.C1=clerp(weaponweld.C1,cf(2,0,0)*angles(math.rad(90),math.rad(0),math.rad(-90)),.9)
end
hum.CameraOffset = vt(0,0,0)
hitb:Destroy()
attack = false
hum.WalkSpeed = Speed
hum.JumpPower = 50
end

function darkslash()
attack = true
hum.WalkSpeed = 80
hum.JumpPower = 0
CFuncs["Sound"].Create("rbxassetid://1368598393", rarmor, 2, 1)
CFuncs["Sound"].Create("rbxassetid://1368583274", rarmor, 2.5, 1)
for x = 0, 1 do
CFuncs["Sound"].Create("rbxassetid://200633108", rarmor, 2, 1.05)
CFuncs["Sound"].Create("rbxassetid://234365573", rarmor, 2.5, 1.025)
for i = 0, 1, 0.6 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.25,0)*angles(math.rad(0),math.rad(0),math.rad(-60)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(60)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(-15),math.rad(180),math.rad(90)),.3)
end
end
wait(0.06)
local hitb = CreateParta(m,1,1,"SmoothPlastic",BrickColor.Random())
hitb.Anchored = true
hitb.CFrame = root.CFrame + root.CFrame.lookVector*8
hitb.CFrame = hitb.CFrame*CFrame.new(0,1,0)
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
for i = 0, 24 do
end
CFuncs["Sound"].Create("rbxassetid://313205954", root, 4,1)
CFuncs["Sound"].Create("rbxassetid://1368637781", rarmor, 4,1)
CFuncs["Sound"].Create("rbxassetid://763718160", rarmor, 5, 1.1)
CFuncs["Sound"].Create("rbxassetid://782353443", rarmor, 6, 1)
--CFuncs["Sound"].Create("rbxassetid://1548538202", rarmor, 4,1)
for i = 0, 2, 0.1 do
		swait()
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
hum.CameraOffset = vt(math.random(-10,10)/25,math.random(-10,10)/25,math.random(-10,10)/25)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-20),math.rad(-10)),.9)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.9)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.5,0)*angles(math.rad(0),math.rad(0),math.rad(80)),.9)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(-80)),.9)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(70)),.9)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-60)),.9)
weaponweld.C1=clerp(weaponweld.C1,cf(2,0,0)*angles(math.rad(0),math.rad(180),math.rad(90)),.9)
end
hum.CameraOffset = vt(0,0,0)
hitb:Destroy()
attack = false
hum.WalkSpeed = Speed
hum.JumpPower = 50
end

function smack()
attack = true
hum.WalkSpeed = 3
hum.JumpPower = 0
CFuncs["Sound"].Create("rbxassetid://1368598393", rarmor, 2, 1)
CFuncs["Sound"].Create("rbxassetid://1368583274", rarmor, 2.5, 1)
for x = 0, 1 do
CFuncs["Sound"].Create("rbxassetid://200633108", rarmor, 2, 1.05)
CFuncs["Sound"].Create("rbxassetid://234365573", rarmor, 2.5, 1.025)
for i = 0, 1, 0.6 do
		swait()
	RH.C0=clerp(RH.C0,cf(1,-0.5,-0.5)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-10)),.2)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(-55),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(30),math.rad(0)),.2)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0.25,0)*angles(math.rad(-60),math.rad(0),math.rad(-0)),.3)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(64),math.rad(0),math.rad(0)),.3)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(80)),.3)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(-60)),.3)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0 + -0 * math.sin(sine / 0.5)),math.rad(-90 + -5 * math.sin(sine / 40)),math.rad(0 + -0.1 * math.sin(sine / 0.5))),.3)
end
end
wait(0.08)
local hitb = CreateParta(m,1,1,"SmoothPlastic",BrickColor.Random())
hitb.Anchored = true
hitb.CFrame = root.CFrame + root.CFrame.lookVector*8
hitb.CFrame = hitb.CFrame*CFrame.new(0,1,0)
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
for i = 0, 24 do
end
CFuncs["Sound"].Create("rbxassetid://313205954", root, 4,1)
CFuncs["Sound"].Create("rbxassetid://1368637781", rarmor, 4,1)
CFuncs["Sound"].Create("rbxassetid://763718160", rarmor, 5, 1.1)
CFuncs["Sound"].Create("rbxassetid://782353443", rarmor, 6, 1)
--CFuncs["Sound"].Create("rbxassetid://1548538202", rarmor, 4,1)
for i = 0, 2, 0.1 do
		swait()
MagniDamage(hitb, 8, 92,158, 0, "Normal",153092213)
hum.CameraOffset = vt(math.random(-10,10)/25,math.random(-10,10)/25,math.random(-10,10)/25)
	RH.C0=clerp(RH.C0,cf(1,-1,-0.5)*angles(math.rad(20),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(-20),math.rad(-10)),.9)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(15),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(0)),.9)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.5,0)*angles(math.rad(30),math.rad(0),math.rad(0)),.9)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(10),math.rad(0),math.rad(0)),.9)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(90),math.rad(0),math.rad(70)),.9)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-60)),.9)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0 + -0 * math.sin(sine / 0.5)),math.rad(-90 + -5 * math.sin(sine / 40)),math.rad(0 + -0.1 * math.sin(sine / 0.5))),.3)
end
hum.CameraOffset = vt(0,0,0)
hitb:Destroy()
attack = false
hum.WalkSpeed = Speed
hum.JumpPower = 50
end

function superjump()
attack = true
hum.WalkSpeed = 0
hum.JumpPower = 0
CFuncs["Sound"].Create("rbxassetid://1368637781", root, 7.5, 1)
for i = 0, 2, 0.1 do
		swait()
hum.CameraOffset = vt(math.random(-10,10)/100,math.random(-10,10)/100,math.random(-10,10)/100)
root.Velocity = vt(0,0,0)
	RH.C0=clerp(RH.C0,cf(1,-0.45,-0.45)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(20)),.4)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(40)),.4)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.5,-1)*angles(math.rad(20),math.rad(0),math.rad(0)),.4)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(4),math.rad(0),math.rad(0)),.4)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(40)),.4)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(10),math.rad(0),math.rad(-40)),.4)
end
CFuncs["Sound"].Create("rbxassetid://477843807", root, 7, 1.05)
local lat1 = Instance.new("Attachment",larm)
lat1.Position = Vector3.new(1,-1,0.5)
local lat2 = Instance.new("Attachment",larm)
lat2.Position = Vector3.new(-1,-1,-0.5)
local rat1 = Instance.new("Attachment",rarm)
rat1.Position = Vector3.new(1,-1,-0.5)
local rat2 = Instance.new("Attachment",rarm)
rat2.Position = Vector3.new(-1,-1,0.5)
local tl1 = Instance.new('Trail',larm)
tl1.Attachment0 = lat1
tl1.Attachment1 = lat2
tl1.Texture = "http://www.roblox.com/asset/?id=1049219073"
tl1.LightEmission = 1
tl1.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1),NumberSequenceKeypoint.new(0.05, 0),NumberSequenceKeypoint.new(1, 1)})
tl1.Color = ColorSequence.new(BrickColor.new('Royal purple').Color,BrickColor.new('Royal purple').Color)
tl1.Lifetime = 5
local tl2 = tl1:Clone()
tl2.Attachment0 = rat1
tl2.Attachment1 = rat2
tl2.Parent = rarm
hum.JumpPower = 20
hum.Jump = true
swait()
hum.JumpPower = 0
root.Velocity = vt(0,250,0) + root.CFrame.lookVector*250
for i = 0, 49 do
end
coroutine.resume(coroutine.create(function()
for i = 0, 2, 0.1 do
swait()
hum.CameraOffset = vt(math.random(-10,10)/50,math.random(-10,10)/50,math.random(-10,10)/50)
end
hum.CameraOffset = vt(0,0,0)
wait(3)
tl1.Enabled = false
tl2.Enabled = false
game:GetService("Debris"):AddItem(tl1, 5)
game:GetService("Debris"):AddItem(tl2, 5)
game:GetService("Debris"):AddItem(rat1, 5)
game:GetService("Debris"):AddItem(rat2, 5)
game:GetService("Debris"):AddItem(lat1, 5)
game:GetService("Debris"):AddItem(lat2, 5)
end))
CFuncs["Sound"].Create("rbxassetid://1295446488", root, 10, 1)
for i = 0, 3, 0.1 do
		swait()
RH.C0=clerp(RH.C0,cf(1,-0.45,-0.45)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(-20)),.4)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3),math.rad(0),math.rad(30)),.4)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.75,0)*angles(math.rad(40),math.rad(0),math.rad(0)),.4)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-20),math.rad(0),math.rad(0)),.4)
RW.C0=clerp(RW.C0,cf(1.45,0.5,0.1)*angles(math.rad(-30),math.rad(0),math.rad(20)),.4)
LW.C0=clerp(LW.C0,cf(-1.45,0.5,0.1)*angles(math.rad(-30),math.rad(0),math.rad(-20)),.4)
end
coroutine.resume(coroutine.create(function()
for i = 0, 54 do
swait()
end
end))
attack = false
if equipped == false then
hum.WalkSpeed = 16
else
hum.WalkSpeed = Speed
end
hum.JumpPower = 50
end
------------------


local attacktype = 1
mouse.Button1Down:connect(function()
if equipped == true then
  if attack == false and attacktype == 1 then
    attacktype = 2
    attackone()
  elseif attack == false and attacktype == 2 then
    attacktype = 3
    attacktwo()
  elseif attack == false and attacktype == 3 then
    attacktype = 1
    attackthree()
  --[[elseif attack == false and attacktype == 4 then
    attacktype = 1
    --attackfour()]]--
  end
end
end)
mouse.KeyDown:connect(function(k)
if k == "f" and attack == false and equipped == false then
	maybe:FindFirstChildOfClass("AlignOrientation").Attachment1 = rarmor.Attachment
    maybe:FindFirstChildOfClass("AlignPosition").Attachment1 = rarmor.Attachment2

    rarmor.Attachment2.Position = Vector3.new(-1.5, -0.100, 0)
    rarmor.Attachment.Rotation = Vector3.new(-1.1, 2, 5000)
	equip()
elseif k == "f" and attack == false and equipped == true then
	maybe:FindFirstChildOfClass("AlignOrientation").Attachment1 = playerss.Torso.WaistBackAttachment
    maybe:FindFirstChildOfClass("AlignPosition").Attachment1 = playerss.Torso.WaistBackAttachment

    playerss.Torso.WaistBackAttachment.Position = Vector3.new(-0, 0.26, 0.6)
    playerss.Torso.WaistBackAttachment.Orientation = Vector3.new(-4.16, -179.28, 160.7)
   unequip()
end
if k == "y" and attack == false and equipped == false then
	maybe:FindFirstChildOfClass("AlignOrientation").Attachment1 = rarmor.Attachment
    maybe:FindFirstChildOfClass("AlignPosition").Attachment1 = rarmor.Attachment2

    rarmor.Attachment2.Position = Vector3.new(-1.25, 0.28, -0)
    rarmor.Attachment.Rotation = Vector3.new(-0, -0, -290)
elseif k == "y" and attack == false and equipped == true then
	maybe:FindFirstChildOfClass("AlignOrientation").Attachment1 = playerss.Torso.WaistBackAttachment
    maybe:FindFirstChildOfClass("AlignPosition").Attachment1 = playerss.Torso.WaistBackAttachment

    playerss.Torso.WaistBackAttachment.Position = Vector3.new(-0, 0.26, 0.6)
    playerss.Torso.WaistBackAttachment.Orientation = Vector3.new(-4.16, -179.28, 125.7)
end
if k == "2" and attack == false then
       hum.WalkSpeed = 40
       Speed = 40
       kan.Pitch = 0.92
       kan.SoundId = "rbxassetid://183142252"
		BanishMode = 2
	end

if k == "1" and attack == false then
      hum.WalkSpeed = 24
      Speed = 24
      kan.Pitch = 0.91
      kan.SoundId = "rbxassetid://5801326053"
		BanishMode = 1
	end

if k == "3" and attack == false then
      hum.WalkSpeed = 13.8
      Speed = 13.8
      kan.Pitch = 0.8
      kan.SoundId = "rbxassetid://4565857495"
		BanishMode = 4
	end

if k == "4" and attack == false then
      hum.WalkSpeed = 8
      Speed = 8
      kan.Pitch = 0.9
      kan.SoundId = "rbxassetid://4466439348"
		BanishMode = 5
	end

if k == "5" and attack == false then
      hum.WalkSpeed = 35
      Speed = 35
      kan.Pitch = 1
      kan.SoundId = "rbxassetid://264721135"
		BanishMode = 7
	end

if k == "r" and attack == false then
superjump()
end
if k == "v" and attack == false then
g1 = Instance.new("BodyGyro", Root)
g1.D = 175
g1.P = 20000
g1.MaxTorque = Vector3.new(0,9000,0)
g1.CFrame = CFrame.new(playerss:FindFirstChild("HumanoidRootPart").Position,mouse.Hit.p)
game:GetService("Debris"):AddItem(g1,.05)
playerss:FindFirstChild("HumanoidRootPart").CFrame = CFrame.new(mouse.Hit.p) * CFrame.new(0,3.3,0)
end
plr.Chatted:connect(function(message)
if message == "/sit" and attack == false then
Speed = 0
hum.WalkSpeed = 0
      BanishMode = 2000
    end

if message == "/glitch" and attack == false and BanishMode == 5 then
Speed = 8
hum.WalkSpeed = 8
kan.Pitch = 0.6
      BanishMode = 1000
      kan.Pitch = 0.6
wait(0.02)
      kan.Pitch = 0.5
wait(0.02)
kan.Pitch = 0.467
    end

if message:sub(1,3) == "id/" then
ORGID = message:sub(4)
kan.TimePosition = 0
kan:Play()
elseif message:sub(1,6) == "pitch/" then
ORPIT = message:sub(7)
elseif message:sub(1,4) == "vol/" then
ORVOL = message:sub(5)
elseif message:sub(1,5) == "skip/" then
kan.TimePositcion = message:sub(6)
end
end)
if equipped == true then

if k == "z" and attack == false then
spinnyblade()
end


if k == "x" and attack == false then
eightbitmegablade()
end
if k == "q" and attack == false and BanishMode == 2 then
axeslash()
end
if k == "q" and attack == false and BanishMode == 4 then
darkslash()
end
if k == "q" and attack == false and BanishMode == 5 then
smack()
end
if k == "e" and attack == false and BanishMode == 4 then
darkspin()
end
if k == "c" and attack == false then
bladespinagain()
end
end
if k == "l" and muter == false then
muter = true
kan.Volume = 0
elseif k == "l" and muter == true then
muter = false
if not NoSound then
	kan.Volume = 1.25
end
end
end)


idleanim=.25
while true do
swait()
if muter == false then
if not NoSound then
	kan.Volume = ORVOL
end
else
kan.Volume = 6
end

  sine = sine + change
local torvel=(RootPart.Velocity*Vector3.new(1,0,1)).magnitude 
local velderp=RootPart.Velocity.y
hitfloor,posfloor=rayCast(RootPart.Position,(CFrame.new(RootPart.Position,RootPart.Position - Vector3.new(0,1,0))).lookVector,4,Character)
if equipped==true or equipped==false then
if attack==false then
idle=idle+1
else
idle=0
end
if idle>=500 then
if attack==false then
--Sheath()
end
end
if RootPart.Velocity.y > 1 and hitfloor==nil then 
Anim="Jump"
if attack==false then
RH.C0=clerp(RH.C0,cf(1,-0.35 - 0.05 * math.cos(sine / 25),-0.75)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-5),math.rad(0),math.rad(-20)),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 - 0.05 * math.cos(sine / 25),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-5),math.rad(0),math.rad(20)),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0 + 0.05 * math.cos(sine / 25))*angles(math.rad(-tors.Velocity.Y/6),math.rad(0),math.rad(0)),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-2.5),math.rad(0),math.rad(0)),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.1 * math.cos(sine / 25),0)*angles(math.rad(-5),math.rad(0),math.rad(25)),.1)
LW.C0=clerp(LW.C0,cf(-1.45,0.5 + 0.1 * math.cos(sine / 25),0)*angles(math.rad(-5),math.rad(0),math.rad(-25)),.1)
if equipped == false then
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(130),math.rad(0)),.3)
end
end
elseif RootPart.Velocity.y < -1 and hitfloor==nil then 
Anim="Fall"
if attack==false then
RH.C0=clerp(RH.C0,cf(1,-0.35 - 0.05 * math.cos(sine / 25),-0.75)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-5),math.rad(0),math.rad(-20)),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 - 0.05 * math.cos(sine / 25),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-5),math.rad(0),math.rad(20)),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0 + 0.05 * math.cos(sine / 25))*angles(math.rad(-tors.Velocity.Y/6),math.rad(0),math.rad(0)),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2.5),math.rad(0),math.rad(0)),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.1 * math.cos(sine / 25),0)*angles(math.rad(-15),math.rad(0),math.rad(55)),.1)
LW.C0=clerp(LW.C0,cf(-1.45,0.5 + 0.1 * math.cos(sine / 25),0)*angles(math.rad(-15),math.rad(0),math.rad(-55)),.1)
if equipped == false then
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(130),math.rad(0)),.3)
end
end
elseif torvel<1 and hitfloor~=nil then
Anim="Idle"
if attack==false and BanishMode == 1 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 20)  - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3 + 2 * math.cos(sine / 40)),math.rad(-15),math.rad(0 + 2 * math.cos(sine / 20))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 20) - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3 - 2 * math.cos(sine / 40)),math.rad(1),math.rad(0 - 2 * math.cos(sine / 20))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.02 * math.cos(sine / 40),-0.05 - 0.05 * math.cos(sine / 20))*angles(math.rad(0 + 2 * math.cos(sine / 20)),math.rad(0 + 2 * math.cos(sine / 40)),math.rad(30 + 3 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2),math.rad(0 - 7 * math.cos(sine / 40)),math.rad(-30 - 3 * math.cos(sine / 40))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-6 + 5 * math.cos(sine / 26)),math.rad(-10 - 6 * math.cos(sine / 24)),math.rad(13 - 5 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.4,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-13 - 1 * math.cos(sine / 25)),math.rad(10 + 2 * math.cos(sine / 24)),math.rad(10 + 2 * math.cos(sine / 34))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
RH.C0=clerp(RH.C0,cf(1,-0.5 + -0.266 * math.sin(sine / 20)  - 0.05 * math.sin(sine / 40),-0.25)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-6 + 4 * math.cos(sine / 40)),math.rad(0 - 8 * math.cos(sine / 40)),math.rad(-10 + 5 * math.cos(sine / 20) - 6 * math.cos(sine / 40))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + -0.266 * math.sin(sine / 20) - 0.05 * math.sin(sine / 40),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-6 - 4 * math.cos(sine / 40)),math.rad(10 - 8 * math.cos(sine / 40)),math.rad(10 - 5 * math.cos(sine / 20) - 3 * math.cos(sine / 40))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.05 * math.cos(sine / 40),1 - 0.266 * math.cos(sine / 20))*angles(math.rad(6 + -5 * math.cos(sine / 20)),math.rad(0 + 5 * math.cos(sine / 40)),math.rad(-20 + 16 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(20 - 4 * math.sin(sine / 42)),math.rad(0 - 4 * math.sin(sine / 40)),math.rad(20 - 30 * math.sin(sine / 40))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.3 * math.sin(sine / 20),0.1)*angles(math.rad(-13 + 3 * math.cos(sine / 26)),math.rad(-20 - 3 * math.cos(sine / 24)),math.rad(20 - 5 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.45,0.5 + 0.3 * math.sin(sine / 20),0.1)*angles(math.rad(-13 - 3 * math.cos(sine / 25)),math.rad(10 + 3 * math.cos(sine / 24)),math.rad(-10 + 5 * math.cos(sine / 34))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(190 + -90 * math.sin(sine / 40)),math.rad(0)),.3)
end
end
if attack==false and BanishMode == 7 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 20)  - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3 + 2 * math.cos(sine / 40)),math.rad(-15),math.rad(0 + 2 * math.cos(sine / 20))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 20) - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3 - 2 * math.cos(sine / 40)),math.rad(1),math.rad(0 - 2 * math.cos(sine / 20))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.02 * math.cos(sine / 40),-0.05 - 0.05 * math.cos(sine / 20))*angles(math.rad(0 + 2 * math.cos(sine / 20)),math.rad(0 + 2 * math.cos(sine / 40)),math.rad(30 + 3 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(2),math.rad(0 - 7 * math.cos(sine / 40)),math.rad(-30 - 3 * math.cos(sine / 40))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-6 + 5 * math.cos(sine / 26)),math.rad(-10 - 6 * math.cos(sine / 24)),math.rad(13 - 5 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.4,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-13 - 1 * math.cos(sine / 25)),math.rad(10 + 2 * math.cos(sine / 24)),math.rad(10 + 2 * math.cos(sine / 34))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(190 + -600 * math.sin(sine / 40))),.3)
else
RH.C0=clerp(RH.C0,cf(1,-0.5 + -0.266 * math.sin(sine / 20)  - 0.05 * math.sin(sine / 40),-0.25)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-6 + 4 * math.cos(sine / 40)),math.rad(0 - 8 * math.cos(sine / 40)),math.rad(-10 + 5 * math.cos(sine / 20) - 6 * math.cos(sine / 40))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + -0.266 * math.sin(sine / 20) - 0.05 * math.sin(sine / 40),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-6 - 4 * math.cos(sine / 40)),math.rad(10 - 8 * math.cos(sine / 40)),math.rad(10 - 5 * math.cos(sine / 20) - 3 * math.cos(sine / 40))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.05 * math.cos(sine / 40),1 - 0.266 * math.cos(sine / 20))*angles(math.rad(6 + -5 * math.cos(sine / 20)),math.rad(0 + 5 * math.cos(sine / 40)),math.rad(10 + 16 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(1+math.random(-10,10)), math.rad(0+math.random(-10,10)), math.rad(0+math.random(-10,10))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.3 * math.sin(sine / 20),0.1)*angles(math.rad(10 + 3 * math.cos(sine / 26)),math.rad(8 - 3 * math.cos(sine / 24)),math.rad(20 - 5 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.45,0.5 + 0.3 * math.sin(sine / 20),0.1)*angles(math.rad(10 - 3 * math.cos(sine / 25)),math.rad(10 + 3 * math.cos(sine / 24)),math.rad(-10 + 5 * math.cos(sine / 34))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0.5 + 5 * math.cos(sine / 30),0,-1.5)*angles(math.rad(0),math.rad(0),math.rad(190 + -800 * math.sin(sine / 40))),.3)
end
end
if attack==false and BanishMode == 2 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 20)  - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3 + 2 * math.cos(sine / 40)),math.rad(-15),math.rad(0 + 2 * math.cos(sine / 20))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 20) - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3 - 2 * math.cos(sine / 40)),math.rad(1),math.rad(0 - 2 * math.cos(sine / 20))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.02 * math.cos(sine / 40),-0.05 - 0.05 * math.cos(sine / 20))*angles(math.rad(0 + 2 * math.cos(sine / 20)),math.rad(0 + 2 * math.cos(sine / 40)),math.rad(30 + 3 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-10 - 15 * math.cos(sine / 0.5)),math.rad(5 - 15 * math.cos(sine / 0.5)),math.rad(20 - 20 * math.cos(sine / 0.5))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-6 + 5 * math.cos(sine / 26)),math.rad(-10 - 6 * math.cos(sine / 24)),math.rad(13 - 5 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.4,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-13 - 1 * math.cos(sine / 25)),math.rad(10 + 2 * math.cos(sine / 24)),math.rad(10 + 2 * math.cos(sine / 34))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 20)  - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-6 + 4 * math.cos(sine / 40)),math.rad(0 - 8 * math.cos(sine / 40)),math.rad(10 + -5 * math.cos(sine / 40) - 6 * math.cos(sine / 40))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 20) - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-6 - 4 * math.cos(sine / 40)),math.rad(10 - 8 * math.cos(sine / 40)),math.rad(-10 - -5 * math.cos(sine / 40) - 3 * math.cos(sine / 40))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.06 * math.cos(sine / 40),-0.05 - 0.08 * math.cos(sine / 20))*angles(math.rad(22 + -5 * math.cos(sine / 20)),math.rad(1 + 0.5 * math.cos(sine / 40)),math.rad(-10 + 8 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-10 - 15 * math.cos(sine / 0.5)),math.rad(5 - 15 * math.cos(sine / 0.5)),math.rad(20 - 20 * math.cos(sine / 0.5))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.155 * math.sin(sine / 20),0.1)*angles(math.rad(25 + 3 * math.cos(sine / 26)),math.rad(30 - 3 * math.cos(sine / 24)),math.rad(60 - 5 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.45,0.5 + 0.155 * math.sin(sine / 20),0.1)*angles(math.rad(25 - 3 * math.cos(sine / 25)),math.rad(10 + 3 * math.cos(sine / 24)),math.rad(-10 + 5 * math.cos(sine / 34))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(180 + -360 * math.sin(sine / 40)),math.rad(0)),.3)
end
end

if attack==false and BanishMode == 4 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 20)  - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3 + 2 * math.cos(sine / 40)),math.rad(-15),math.rad(0 + 2 * math.cos(sine / 20))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 20) - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3 - 2 * math.cos(sine / 40)),math.rad(1),math.rad(0 - 2 * math.cos(sine / 20))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.02 * math.cos(sine / 40),-0.05 - 0.05 * math.cos(sine / 20))*angles(math.rad(0 + 2 * math.cos(sine / 20)),math.rad(0 + 2 * math.cos(sine / 40)),math.rad(30 + 3 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-10 - 15 * math.cos(sine / 0.5)),math.rad(5 - 15 * math.cos(sine / 0.5)),math.rad(20 - 20 * math.cos(sine / 0.5))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-6 + 5 * math.cos(sine / 26)),math.rad(-10 - 6 * math.cos(sine / 24)),math.rad(13 - 5 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.4,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-13 - 1 * math.cos(sine / 25)),math.rad(10 + 2 * math.cos(sine / 24)),math.rad(10 + 2 * math.cos(sine / 34))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
RH.C0=clerp(RH.C0,cf(1,-1 + -0.05 * math.cos(sine / 80)  - 0.02 * math.cos(sine / 80),-0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-6 + 4 * math.cos(sine / 80)),math.rad(10 - 8 * math.cos(sine / 80)),math.rad(10 + -5 * math.cos(sine / 80) - 6 * math.cos(sine / 80))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 80) - 0.02 * math.cos(sine / 80),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-6 - 4 * math.cos(sine / 80)),math.rad(10 - 8 * math.cos(sine / 80)),math.rad(-10 - -5 * math.cos(sine / 80) - 6 * math.cos(sine / 80))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.1 * math.cos(sine / 80),0 - 0.095 * math.cos(sine / 80),-0 - 0.122 * math.cos(sine / 40))*angles(math.rad(12.6 + -5 * math.cos(sine / 40)),math.rad(1 + 0.5 * math.cos(sine / 40)),math.rad(-10 + 8 * math.cos(sine / 80))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(18+math.random(-30,30)), math.rad(0+math.random(-30,30)), math.rad(0+math.random(-30,30))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.2 * math.sin(sine / 40),0.1)*angles(math.rad(20+math.random(-10,10)), math.rad(10+math.random(-10,10)), math.rad(12+math.random(-10,10))),.1)
LW.C0=clerp(LW.C0,cf(-1.45,0.5 + 0.2 * math.sin(sine / 40),0.1)*angles(math.rad(25 - 3 * math.cos(sine / 25)),math.rad(30 + 3 * math.sin(sine / 40)),math.rad(-10 + 5 * math.cos(sine / 34))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0 + -0.1 * math.sin(sine / 0.5)),math.rad(180 + -90 * math.sin(sine / 40)),math.rad(0 + -0.1 * math.sin(sine / 0.5))),.3)
end
end

if attack==false and BanishMode == 5 then
if equipped == false then
kan.Pitch = 0.95
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 20)  - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3 + 2 * math.cos(sine / 40)),math.rad(-15),math.rad(0 + 2 * math.cos(sine / 20))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 20) - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3 - 2 * math.cos(sine / 40)),math.rad(1),math.rad(0 - 2 * math.cos(sine / 20))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.02 * math.cos(sine / 40),-0.05 - 0.05 * math.cos(sine / 20))*angles(math.rad(0 + 2 * math.cos(sine / 20)),math.rad(0 + 2 * math.cos(sine / 40)),math.rad(-30 + 10 * math.sin(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(1+math.random(-10,10)), math.rad(0+math.random(-10,10)), math.rad(0+math.random(-10,10))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-6 + 5 * math.cos(sine / 26)),math.rad(-10 - 6 * math.cos(sine / 24)),math.rad(13 - 5 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.4,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-13 - 1 * math.cos(sine / 25)),math.rad(10 + 2 * math.cos(sine / 24)),math.rad(10 + 2 * math.cos(sine / 34))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
kan.Pitch = 0.778
RH.C0=clerp(RH.C0,cf(1,-1 + -0.255 * math.cos(sine / 40)  - 0 * math.cos(sine / 40),-0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-8 + 0 * math.cos(sine / 80)),math.rad(0 - 0 * math.cos(sine / 80)),math.rad(0 + -0 * math.cos(sine / 80) - 0 * math.cos(sine / 40))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + -0.255 * math.cos(sine / 40) - 0 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-8 - 0 * math.cos(sine / 80)),math.rad(0 - 0 * math.cos(sine / 80)),math.rad(0 - -0 * math.cos(sine / 80) - 0 * math.cos(sine / 40))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf( 0 + 0 * math.cos(sine / 40),0 - 0 * math.cos(sine / 40),0 - -0.255 * math.cos(sine / 40))*angles(math.rad(1 + -0.1 * math.cos(sine / 40)),math.rad(1 + 0.1 * math.cos(sine / 40)),math.rad(-1 + 2 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(1+math.random(-10,10)), math.rad(0+math.random(-10,10)), math.rad(0+math.random(-10,10))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.585 + -0.277 * math.sin(sine / 40),0.1)*angles(math.rad(-20+math.random(-10,10)), math.rad(10+math.random(-10,10)), math.rad(-150+math.random(-10,10))),.1)
LW.C0=clerp(LW.C0,cf(-1.45,0.5 + -0.277 * math.sin(sine / 40),0.1)*angles(math.rad(5 - 3 * math.cos(sine / 25)),math.rad(0 + 10 * math.sin(sine / 40)),math.rad(-12 + -11 * math.cos(sine / 40))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0 + -0 * math.sin(sine / 0.5)),math.rad(-90 + -5 * math.sin(sine / 40)),math.rad(0 + -0.1 * math.sin(sine / 0.5))),.3)
end
end

if attack==false and BanishMode == 1000 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 20)  - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3 + 2 * math.cos(sine / 40)),math.rad(-15),math.rad(0 + 2 * math.cos(sine / 20))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 20) - 0.02 * math.cos(sine / 40),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-3 - 2 * math.cos(sine / 40)),math.rad(1),math.rad(0 - 2 * math.cos(sine / 20))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.02 * math.cos(sine / 40),-0.05 - 0.05 * math.cos(sine / 20))*angles(math.rad(0 + 2 * math.cos(sine / 20)),math.rad(0 + 2 * math.cos(sine / 40)),math.rad(30 + 3 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(1+math.random(-10,10)), math.rad(0+math.random(-10,10)), math.rad(0+math.random(-10,10))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-6 + 5 * math.cos(sine / 26)),math.rad(-10 - 6 * math.cos(sine / 24)),math.rad(13 - 5 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.4,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-13 - 1 * math.cos(sine / 25)),math.rad(10 + 2 * math.cos(sine / 24)),math.rad(10 + 2 * math.cos(sine / 34))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
RH.C0=clerp(RH.C0,cf(1 + 0 * (1+math.random(-10,10)),-1 + -0.255 * (1+math.random(-10,10))  - 0 * (1+math.random(-10,10)),-0 + 0 * (1+math.random(-10,10)))*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-8 + 0 * math.cos(sine / 80)),math.rad(0 - 0 * math.cos(sine / 80)),math.rad(0 + -0 * math.cos(sine / 80) - 0 * math.cos(sine / 40))),.1)
LH.C0=clerp(LH.C0,cf(-1 + 0 * (1+math.random(-10,10)),-1 + -0.255 * (1+math.random(-10,10)) - 0 * (1+math.random(-10,10)),0 + 0 * (1+math.random(-10,10)))*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(-8 - 0 * math.cos(sine / 80)),math.rad(0 - 0 * math.cos(sine / 80)),math.rad(0 - -0 * math.cos(sine / 80) - 0 * math.cos(sine / 40))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf( 0 + 0 * (1+math.random(-10,10)),0 - 0 * (1+math.random(-10,10)),0 - -0.255 * (1+math.random(-2.5,10)))*angles(math.rad(1 + -0.1 * math.cos(sine / 40)),math.rad(1 + 0.1 * math.cos(sine / 40)),math.rad(-10 + 2 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(1+math.random(-10,10)), math.rad(0+math.random(-10,10)), math.rad(0+math.random(-10,10))),.1)
RW.C0=clerp(RW.C0,cf(1.45 + 0 * (1+math.random(-10,10)),0.585 + -0.277 * (1+math.random(-10,10)),-0.15 - 0 * (1+math.random(-10,10)))*angles(math.rad(-20+math.random(-10,10)), math.rad(10+math.random(-10,10)), math.rad(-150+math.random(-10,10))),.1)
LW.C0=clerp(LW.C0,cf(-1.45 + 0 * (1+math.random(-10,10)),0.5 + -0.277 * (1+math.random(-10,10)),0.1 - 0 * (1+math.random(-10,10)))*angles(math.rad(5 - 3 * math.cos(sine / 25)),math.rad(0 + 10 * math.sin(sine / 40)),math.rad(-12 + -11 * math.cos(sine / 40))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0 + -0 * math.sin(sine / 0.5)),math.rad(-90 + -5 * math.sin(sine / 40)),math.rad(0 + -0.1 * math.sin(sine / 0.5))),.3)
end
end

if attack==false and BanishMode == "KAR" then
RH.C0=clerp(RH.C0,cf(1,0.255 + 0.05 * math.cos(sine / 20)  - 0.05 * math.cos(sine / 40),-0.8)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(-3 + 2 * math.cos(sine / 40)),math.rad(-15),math.rad(0 + 2 * math.cos(sine / 20))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1.45 + 0.05 * math.cos(sine / 20) - 0.02 * math.cos(sine / 40),0)*angles(math.rad(-0),math.rad(-90),math.rad(90))*angles(math.rad(-3 - 2 * math.cos(sine / 40)),math.rad(1),math.rad(0 - 2 * math.cos(sine / 20))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.02 * math.cos(sine / 40),-1.25 - 0.05 * math.cos(sine / 20))*angles(math.rad(0 + 2 * math.cos(sine / 20)),math.rad(0 + 2 * math.cos(sine / 40)),math.rad(0 + 3 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-0 - 40 * math.cos(sine / 0.5)),math.rad(2 - 45 * math.cos(sine / 0.5)),math.rad(0 - 40 * math.cos(sine / 0.5))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(-10 + 5 * math.cos(sine / 26)),math.rad(-10 - 6 * math.cos(sine / 24)),math.rad(-20 - 20 * math.cos(sine / 1))),.1)
LW.C0=clerp(LW.C0,cf(-1.4,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(180 - 1 * math.cos(sine / 25)),math.rad(10 + 2 * math.cos(sine / 24)),math.rad(20 + 2 * math.cos(sine / 1))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(190 + -90 * math.sin(sine / 40)),math.rad(0)),.3)
end

if attack==false and BanishMode == 2000 then
RH.C0=clerp(RH.C0,cf(1,-1.45 + 0.05 * math.cos(sine / 20)  - 0.05 * math.cos(sine / 40),0)*angles(math.rad(10),math.rad(90),math.rad(90))*angles(math.rad(-3 + 2 * math.cos(sine / 40)),math.rad(-15),math.rad(0 + 2 * math.cos(sine / 20))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1.45 + 0.05 * math.cos(sine / 20) - 0.05 * math.cos(sine / 40),0)*angles(math.rad(10),math.rad(-90),math.rad(-90))*angles(math.rad(-3 - 2 * math.cos(sine / 40)),math.rad(1),math.rad(0 - 2 * math.cos(sine / 20))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0 + 0.02 * math.cos(sine / 40),0 - 0.02 * math.cos(sine / 40),-1.75 - 0.05 * math.cos(sine / 20))*angles(math.rad(0 + 2 * math.cos(sine / 20)),math.rad(0 + 2 * math.cos(sine / 40)),math.rad(0 + 3 * math.cos(sine / 40))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-0 - 2 * math.cos(sine / 8)),math.rad(0 - 2 * math.cos(sine / 16)),math.rad(0 - 2 * math.cos(sine / 8))),.1)
RW.C0=clerp(RW.C0,cf(1.45,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(10 + 5 * math.cos(sine / 26)),math.rad(-10 - 4 * math.cos(sine / 24)),math.rad(20 - 2 * math.cos(sine / 24))),.1)
LW.C0=clerp(LW.C0,cf(-1.4,0.5 + 0.05 * math.cos(sine / 28),0.1)*angles(math.rad(10 - 5 * math.cos(sine / 25)),math.rad(10 + 4 * math.cos(sine / 24)),math.rad(-20 + 2 * math.cos(sine / 24))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(190 + -90 * math.sin(sine / 40)),math.rad(0)),.3)
end

elseif torvel>2 and torvel<42 and hitfloor~=nil then
Anim="Walk"
if attack==false and BanishMode == 1 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.05,-0.05 + 0.05 * math.cos(sine / 4))*angles(math.rad(5 + 3 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 8))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-5 - 5 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - hed.RotVelocity.Y*1.5 + 10 * math.cos(sine / 8))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.25 * math.cos(sine / 8))*angles(math.rad(0 - 50 * math.cos(sine / 8)),math.rad(0),math.rad(5 - 10 * math.cos(sine / 4))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.25 * math.cos(sine / 8))*angles(math.rad(0 + 50 * math.cos(sine / 8)),math.rad(0),math.rad(-5 + 10 * math.cos(sine / 4))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
RH.C0=clerp(RH.C0,cf(1,-0.5 - -0.266 * math.sin(sine / 22),-0.6)*angles(math.rad(-10),math.rad(90),math.rad(-20))*angles(math.rad(0),math.rad(0),math.rad(-4 + 2 * math.sin(sine / 22))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 - -0.266 * math.sin(sine / 22),-0)*angles(math.rad(10),math.rad(-90),math.rad(20))*angles(math.rad(0),math.rad(0),math.rad(6 + 2 * math.sin(sine / 22))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.3,1.1 + 0.34 * math.cos(sine / 22))*angles(math.rad(45 - 2 * math.sin(sine / 22)),math.rad(0 + root.RotVelocity.Y*1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 22))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-2.5 + 4 * math.cos(sine / 22)),math.rad(0 + root.RotVelocity.Y*1.5),math.rad(0 - hed.RotVelocity.Y*1.5 + 10 * math.cos(sine / 22))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.255 * math.sin(sine / 22))*angles(math.rad(-10),math.rad(0),math.rad(15 - 2 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.255 * math.sin(sine / 22))*angles(math.rad(0 + 3 * math.sin(sine / 22)),math.rad(0),math.rad(-5 + 3 * math.sin(sine / 22))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(190 + -90 * math.sin(sine / 40)),math.rad(0)),.3)
end
end
if attack==false and BanishMode == 7 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.05,-0.05 + 0.05 * math.cos(sine / 4))*angles(math.rad(5 + 3 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 8))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-5 - 5 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - hed.RotVelocity.Y*1.5 + 10 * math.cos(sine / 8))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.25 * math.cos(sine / 8))*angles(math.rad(0 - 50 * math.cos(sine / 8)),math.rad(0),math.rad(5 - 10 * math.cos(sine / 4))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.25 * math.cos(sine / 8))*angles(math.rad(0 + 50 * math.cos(sine / 8)),math.rad(0),math.rad(-5 + 10 * math.cos(sine / 4))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
RH.C0=clerp(RH.C0,cf(1,-0.5 - -0.266 * math.sin(sine / 22),-0.6)*angles(math.rad(-10),math.rad(90),math.rad(-20))*angles(math.rad(0),math.rad(0),math.rad(-4 + 2 * math.sin(sine / 22))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 - -0.266 * math.sin(sine / 22),-0)*angles(math.rad(10),math.rad(-90),math.rad(20))*angles(math.rad(0),math.rad(0),math.rad(6 + 2 * math.sin(sine / 22))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.3,1.1 + 0.34 * math.cos(sine / 22))*angles(math.rad(45 - 2 * math.sin(sine / 22)),math.rad(0 + root.RotVelocity.Y*1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 22))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(1+math.random(-10,10)), math.rad(0+math.random(-10,10)), math.rad(0+math.random(-10,10))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.255 * math.sin(sine / 22))*angles(math.rad(-10),math.rad(0),math.rad(15 - 2 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.255 * math.sin(sine / 22))*angles(math.rad(0 + 3 * math.sin(sine / 22)),math.rad(0),math.rad(-5 + 3 * math.sin(sine / 22))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0.5 + 5 * math.cos(sine / 30),0,-1.5)*angles(math.rad(0),math.rad(0),math.rad(190 + -800 * math.sin(sine / 40))),.3)
end
end
if attack==false and BanishMode == 2 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.05,-0.05 + 0.05 * math.cos(sine / 4))*angles(math.rad(5 + 3 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 8))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-5 - 5 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - hed.RotVelocity.Y*1.5 + 10 * math.cos(sine / 8))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.25 * math.cos(sine / 8))*angles(math.rad(0 - 50 * math.cos(sine / 8)),math.rad(0),math.rad(5 - 10 * math.cos(sine / 4))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.25 * math.cos(sine / 8))*angles(math.rad(0 + 50 * math.cos(sine / 8)),math.rad(0),math.rad(-5 + 10 * math.cos(sine / 4))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
RH.C0=clerp(RH.C0,cf(1,-1 - 0.15 * math.cos(sine / 3),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0),math.rad(0 + 85 * math.cos(sine / 6))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 - 0.15 * math.cos(sine / 3),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0),math.rad(0 + 85 * math.cos(sine / 6))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.3,-0.05 + 0.15 * math.cos(sine / 3))*angles(math.rad(15 - 4 * math.cos(sine / 3)),math.rad(0 + root.RotVelocity.Y*1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 6))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-6 - 15 * math.cos(sine / 0.5)),math.rad(6 - 15 * math.cos(sine / 0.5)),math.rad(10 - 20 * math.cos(sine / 0.5))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.25 * math.cos(sine / 4.6))*angles(math.rad(-40),math.rad(0),math.rad(25 - 2 * math.cos(sine / 34))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.5 * math.cos(sine / 6))*angles(math.rad(0 + 140 * math.cos(sine / 6)),math.rad(0),math.rad(-5 + 20 * math.cos(sine / 3))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(190 + -360 * math.sin(sine / 40)),math.rad(0)),.3)
end
end
if attack==false and BanishMode == 4 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.05,-0.05 + 0.05 * math.cos(sine / 4))*angles(math.rad(5 + 3 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 8))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(20+math.random(-30,30)), math.rad(0+math.random(-30,30)), math.rad(0+math.random(-30,30))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.25 * math.cos(sine / 8))*angles(math.rad(0 - 50 * math.cos(sine / 8)),math.rad(0),math.rad(5 - 10 * math.cos(sine / 4))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.25 * math.cos(sine / 8))*angles(math.rad(0 + 50 * math.cos(sine / 8)),math.rad(0),math.rad(-5 + 10 * math.cos(sine / 4))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 12),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 16)),math.rad(0 + 22 * math.cos(sine / 16))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 12),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 16)),math.rad(0 + 22 * math.cos(sine / 16))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.05,-0.05 + 0.05 * math.cos(sine / 12))*angles(math.rad(10 + 3 * math.cos(sine / 12)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 16))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(30+math.random(-30,30)), math.rad(0+math.random(-30,30)), math.rad(0+math.random(-30,30))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.25 * math.cos(sine / 16))*angles(math.rad(12 - 12 * math.cos(sine / 16)),math.rad(0),math.rad(5 - 10 * math.cos(sine / 8))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.25 * math.cos(sine / 16))*angles(math.rad(12 + 12 * math.cos(sine / 16)),math.rad(0),math.rad(-5 + 10 * math.cos(sine / 8))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(190 + -100 * math.sin(sine / 80)),math.rad(0)),.3)
end
end

if attack==false and BanishMode == 5 then
if equipped == false then
kan.Pitch = 0.95
RH.C0=clerp(RH.C0,cf(1,-1 + 0.2 * math.sin(sine / 8),-0.12 + 0.2 * math.cos(sine / 8))*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + -40 * math.cos(sine / 8))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + -0.2 * math.sin(sine / 8),-0.12 + -0.2 * math.cos(sine / 8))*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + -40 * math.cos(sine / 8))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.05,-0.05 + 0.05 * math.cos(sine / 4))*angles(math.rad(10 + 3 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1),math.rad(0 - root.RotVelocity.Y - 6 * math.sin(sine / 8))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(3+math.random(-10,10)), math.rad(0+math.random(-25,12)), math.rad(0+math.random(-10,10))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + -0.25 * math.sin(sine / 8))*angles(math.rad(12 + 18 * math.cos(sine / 8)),math.rad(0),math.rad(0 + 0 * math.cos(sine / 4))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - -0.25 * math.sin(sine / 8))*angles(math.rad(12 + -18 * math.cos(sine / 8)),math.rad(0),math.rad(0 + 0 * math.cos(sine / 4))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
kan.Pitch = 0.778
RH.C0=clerp(RH.C0,cf(1,-1 + 0.2 * math.sin(sine / 12),-0.12 + 0.2 * math.cos(sine / 12))*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 12)),math.rad(0 + -22 * math.cos(sine / 12))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + -0.2 * math.sin(sine / 12),-0.12 + -0.2 * math.cos(sine / 12))*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 12)),math.rad(0 + -22 * math.cos(sine / 12))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.05,-0.05 + 0.05 * math.cos(sine / 6))*angles(math.rad(10 + 3 * math.cos(sine / 6)),math.rad(0 + root.RotVelocity.Y/0.6),math.rad(0 - root.RotVelocity.Y - 6 * math.sin(sine / 12))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(3+math.random(-10,10)), math.rad(0+math.random(-25,12)), math.rad(0+math.random(-10,10))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + -0.25 * math.sin(sine / 12))*angles(math.rad(12 + 18 * math.cos(sine / 12)),math.rad(0),math.rad(0 + 0 * math.cos(sine / 6))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - -0.25 * math.sin(sine / 12))*angles(math.rad(95 + -3 * math.cos(sine / 12)),math.rad(0),math.rad(0 + 0 * math.cos(sine / 6))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(-90 + -2 * math.sin(sine / 80)),math.rad(0)),.3)
end
end

if attack==false and BanishMode == 1000 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.05,-0.05 + 0.05 * math.cos(sine / 4))*angles(math.rad(5 + 3 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 8))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(20+math.random(-100,100)), math.rad(0+math.random(-100,100)), math.rad(0+math.random(-100,100))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.25 * math.cos(sine / 8))*angles(math.rad(0 - 50 * math.cos(sine / 8)),math.rad(0),math.rad(5 - 10 * math.cos(sine / 4))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.25 * math.cos(sine / 8))*angles(math.rad(0 + 50 * math.cos(sine / 8)),math.rad(0),math.rad(-5 + 10 * math.cos(sine / 4))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
RH.C0=clerp(RH.C0,cf(1 + 0.2 * (1+math.random(-5,5)),-1 + 0.2 * (1+math.random(-5,5)),0 + 0.1 * (1+math.random(-5,5)))*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 18)),math.rad(0 + -22 * math.cos(sine / 18))),.1)
LH.C0=clerp(LH.C0,cf(-1 + 0.2 * (1+math.random(-5,5)),-1 + 0.2 * (1+math.random(-5,5)),0 + -0.1 * (1+math.random(-5,5)))*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 18)),math.rad(0 + -22 * math.cos(sine / 18))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(-0 + 0.05 * (1+math.random(-10,10)),-0.05 + 0.05 * (1+math.random(-10,10)),-0.05 + 0.05 * (1+math.random(-10,10)))*angles(math.rad(10 + 3 * math.cos(sine / 9)),math.rad(0 + root.RotVelocity.Y/1),math.rad(0 - root.RotVelocity.Y - -6 * math.sin(sine / 18))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(3+math.random(-100,100)), math.rad(0+math.random(-100,100)), math.rad(0+math.random(-100,100))),.1)
RW.C0=clerp(RW.C0,cf(1.5 + 0.05 * (1+math.random(-20,20)),0.5 + 0.05 * (1+math.random(-20,20)),0 + 0.05 * (1+math.random(-20,20)))*angles(math.rad(-20+math.random(-10,10)), math.rad(0+math.random(-10,10)), math.rad(215+math.random(-10,10))),.1)
LW.C0=clerp(LW.C0,cf(-1.5 + 0.05 * (1+math.random(-20,20)),0.5 + 0.05 * (1+math.random(-20,20)),0 - -0.25 * (1+math.random(-20,20)))*angles(math.rad(12 + -18 * math.cos(sine / 18)),math.rad(0),math.rad(0 + 0 * math.cos(sine / 9))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(-90 + -2 * math.sin(sine / 80)),math.rad(0)),.3)
end
end

if attack==false and BanishMode == 3 then
if equipped == false then
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.05,-0.05 + 0.05 * math.cos(sine / 4))*angles(math.rad(5 + 3 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 8))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-5 - 5 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - hed.RotVelocity.Y*1.5 + 10 * math.cos(sine / 8))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.25 * math.cos(sine / 8))*angles(math.rad(0 - 50 * math.cos(sine / 8)),math.rad(0),math.rad(5 - 10 * math.cos(sine / 4))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.25 * math.cos(sine / 8))*angles(math.rad(0 + 50 * math.cos(sine / 8)),math.rad(0),math.rad(-5 + 10 * math.cos(sine / 4))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(-3,0,-0.5)*angles(math.rad(0),math.rad(0),math.rad(-40)),.3)
else
RH.C0=clerp(RH.C0,cf(1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 + 0.05 * math.cos(sine / 4),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0 + 5 * math.cos(sine / 8)),math.rad(0 + 45 * math.cos(sine / 8))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.05,-0.05 + 0.05 * math.cos(sine / 4))*angles(math.rad(5 + 3 * math.cos(sine / 4)),math.rad(0 + root.RotVelocity.Y/1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 8))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-0 - 35 * math.cos(sine / 0.5)),math.rad(10 - 30 * math.cos(sine / 0.5)),math.rad(0 - 25 * math.cos(sine / 0.5))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.25 * math.cos(sine / 8))*angles(math.rad(0 - 50 * math.cos(sine / 8)),math.rad(0),math.rad(5 - 10 * math.cos(sine / 4))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.25 * math.cos(sine / 8))*angles(math.rad(0 + 50 * math.cos(sine / 8)),math.rad(0),math.rad(-5 + 10 * math.cos(sine / 4))),.1)
weaponweld.C1=clerp(weaponweld.C1,cf(0,1,0)*angles(math.rad(0),math.rad(190 + -999 * math.sin(sine / 40)),math.rad(0)),.3)
end
end
elseif torvel>=42 and hitfloor~=nil then
Anim="Run"
if attack==false then
RH.C0=clerp(RH.C0,cf(1,-1 - 0.15 * math.cos(sine / 3),0)*angles(math.rad(0),math.rad(90),math.rad(0))*angles(math.rad(0),math.rad(0),math.rad(0 + 85 * math.cos(sine / 6))),.1)
LH.C0=clerp(LH.C0,cf(-1,-1 - 0.15 * math.cos(sine / 3),0)*angles(math.rad(0),math.rad(-90),math.rad(0))*angles(math.rad(0),math.rad(0),math.rad(0 + 85 * math.cos(sine / 6))),.1)
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,-0.3,-0.05 + 0.15 * math.cos(sine / 3))*angles(math.rad(15 - 4 * math.cos(sine / 3)),math.rad(0 + root.RotVelocity.Y*1.5),math.rad(0 - root.RotVelocity.Y - 10 * math.cos(sine / 6))),.1)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*angles(math.rad(-2.5 + 4 * math.cos(sine / 3)),math.rad(0 + root.RotVelocity.Y*1.5),math.rad(0 - hed.RotVelocity.Y*1.5 + 10 * math.cos(sine / 6))),.1)
RW.C0=clerp(RW.C0,cf(1.5,0.5,0 + 0.5 * math.cos(sine / 6))*angles(math.rad(0 - 140 * math.cos(sine / 6)),math.rad(0),math.rad(5 - 20 * math.cos(sine / 3))),.1)
LW.C0=clerp(LW.C0,cf(-1.5,0.5,0 - 0.5 * math.cos(sine / 6))*angles(math.rad(0 + 140 * math.cos(sine / 6)),math.rad(0),math.rad(-5 + 20 * math.cos(sine / 3))),.1)
end
end
end
end
------
