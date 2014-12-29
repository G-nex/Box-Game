Box-Game
========


--# Info
--Created by G_nex, 2014



--New in 0.13:
--Earn money by stepping in a box!
--Pause button


--To do:
--More boxes!
--Randomly spawning boxes
--Randomly spawning power-ups
--# Main
displayMode(FULLSCREEN)

function setup()
    
    --Setup
    gameVersion = 0.13
    song = music("Game Music One:Funk Blue Cube")
    
    --Menu setup
    gameMode = menu
    fade1 = 0
    titleX = WIDTH
    
    --Game setup
    money = 0
    player = Player(WIDTH/2, HEIGHT/2, 100)
    dpad = controls(150, 150, player, 6)
    paused = false
    box1 = Box(WIDTH/2+100,HEIGHT/2-100,100,normal)
end



function draw()
    
    gameMode()
    
end



function menu()
    if fade1 < 200 then
        fade1 = fade1 + 1
    end
        
    if titleX > WIDTH/2 + 110 then
        titleX = titleX - 10
    end
        
    background(0, 0, 0, 255)
    font("HelveticaNeue-CondensedBlack")
    fontSize(110)
    fill(255, 218, 0, 255)
    text("B    x", WIDTH/2-100, HEIGHT/2+220)
    sprite("Cargo Bot:Crate Yellow 2", WIDTH/2-100, HEIGHT/2+220, 90)
    fill(0, 1, 255, 255)
    text("Game", titleX, HEIGHT/2+140)
    fill(255, 255, 255, 255)
    fontSize(20)
    text("Version: "..gameVersion, 70, 50)
    
    fill(fade1, fade1, fade1, 255)
    fontSize(50)
    rect(WIDTH/2-200, HEIGHT/2-50, 400, 100)
    fill(0, 0, 0, 255)
    text("Start", WIDTH/2, HEIGHT/2)
    fill(fade1, fade1, fade1, 255)
    rect(WIDTH/2-200, HEIGHT/2-200, 400, 100)
    fill(0, 0, 0, 255)
    text("Settings", WIDTH/2, HEIGHT/2-150)
    
end



function game()
    background(26, 189, 18, 255)

    sprite("Small World:Dirt Patch",WIDTH/2, HEIGHT/2, 1000)
    player:draw()
    dpad:draw()
    box1:draw()
    cash = math.ceil(money)
    fontSize(40)
    text("Cash:"..cash, 200, HEIGHT-50)
    
    --Pause button
    sprite("Cargo Bot:Claw Arm", WIDTH-50, HEIGHT-50, 10, 40)
    sprite("Cargo Bot:Claw Arm", WIDTH-25, HEIGHT-50, 10, 40)
    
    if paused == true then
        fill(164, 164, 164, 200)
        rect(WIDTH/2-250, HEIGHT/2, 500, 300)
        fill(127, 127, 127, 255)
        rect(WIDTH/2-200, HEIGHT/2+150, 400, 100)
        rect(WIDTH/2-200, HEIGHT/2+20, 400, 100)
        fill(0, 0, 0, 255)
        text("Resume", WIDTH/2, HEIGHT/2+200)
        text("Menu", WIDTH/2, HEIGHT/2+70)
    end
end



function touched(t)
    --Menu
    if gameMode == menu then
        
    if t.x>WIDTH/2-200 and t.x<WIDTH/2+200 and t.y>HEIGHT/2-50 and t.y<HEIGHT/2+50 and t.tapCount == 1 and t.state == ENDED then
            gameMode = game
            song = music("Game Music One:Jungle Rampage")
    end
        
                
    --Game
    elseif gameMode == game then
    
    if paused == false then
    dpad:touched(t)
    if t.x>WIDTH-75 and t.y>HEIGHT-75 then
        music.paused = true
        paused = true
    end
    end
        
    if paused == true then
    if t.x>WIDTH/2-200 and t.x<WIDTH/2+200 and t.y>HEIGHT/2+20 and t.y<HEIGHT/2+120 and t.tapCount == 1 and t.state == ENDED then
        gameMode = menu
        paused = false
        music.paused = false
        song = music("Game Music One:Funk Blue Cube")
    end
    if t.x>WIDTH/2-200 and t.x<WIDTH/2+200 and t.y>HEIGHT/2+150 and t.y<HEIGHT/2+250 and t.tapCount == 1 and t.state == ENDED then
        paused = false
        music.paused = false
    end
    end
          
    end
    
end


--# Box
Box = class()

function Box:init(x, y, size, colr)
    -- you can accept and set parameters here
    self.x = x 
    self.y = y 
    self.size = size
    self.colr = colr
    self.sprite = nil
    
    if self.colr == normal then
        self.sprite = "Cargo Bot:Title Large Crate "..math.random(1,3)
    elseif self.colr == blue then
        self.sprite = "Cargo Bot:Crate Blue "..math.random(1,3)
    elseif self.colr == red then
        self.sprite = "Cargo Bot: Crate Red "..math.random(1,3)
    elseif self.colr == yellow then
        self.sprite = "Cargo Bot: Crate Yellow "..math.random(1,3)
    elseif self.colr == green then
        self.sprite = "Cargo Bot: Crate Green "..math.random(1,3)
    end
end

function Box:draw()
    -- Codea does not automatically call this method
    
    sprite(self.sprite, self.x, self.y, self.size)
    if player.x>self.x-self.size/2 and player.x<self.x+self.size/2 and player.y>self.y-self.size/2 and player.y<self.y+self.size/2 then
        fill(255, 255, 255, 255)
        rect(player.x-75, player.y+50, 150, 40)
        fill(0, 0, 0, 255)
        fontSize(20)
        text("I'm in a box!", player.x, player.y+70)
        fontSize(40)
        money = money+0.01
    end
end

function Box:touched(touch)
    -- Codea does not automatically call this method
end

--# controls
--# controls
controls = class()

function controls:init(x,y,subject,speed)
    -- you can accept and set parameters here
    self.x = x
    self.y = y
    self.subject = subject
    self.speed = speed
    self.dirX = 0
    self.dirY = 0 
    
    self.leftSize = 50
    self.rightSize = 50
    self.downSize = 50
    self.upSize = 50
end

function controls:draw()
    -- Codea does not automatically call this method
    sprite("Cargo Bot:Command Left", self.x-50, self.y, self.leftSize)
    sprite("Cargo Bot:Command Right", self.x+50, self.y, self.rightSize)
    sprite("Cargo Bot:Command Grab",self.x, self.y-50, self.downSize)
    sprite("Cargo Bot:Command Grab", self.x, self.y+50, -self.upSize)
    
    if self.subject.x<=0 then
        self.subject.x=0
    end
    if self.subject.x>=WIDTH then
        self.subject.x=WIDTH
    end
    if self.subject.y<=50 then
        self.subject.y=50
    end
    if self.subject.y>=HEIGHT then
        self.subject.y=HEIGHT
    end
    
    self.subject.x = self.subject.x + self.dirX
    self.subject.y = self.subject.y + self.dirY
end

function controls:touched(t)
    -- Codea does not automatically call this method
        --dpad
    if t.state==BEGAN then
        if t.x>75 and t.x<125 and t.y>125 and t.y<175 then
            self.dirX=-self.speed -- move left
            self.leftSize = 75
        end  
        if t.x>175 and t.x<225 and t.y>125 and t.y<175 then
            self.dirX=self.speed  -- move right
            self.rightSize = 75
        end       
        if t.x>125 and t.x<175 and t.y>175 and t.y<225 then
            self.dirY=self.speed  -- move up
            self.upSize = 75
        end       
        if t.x>125 and t.x<175 and t.y>75 and t.y<125 then
            self.dirY=-self.speed -- move down
            self.downSize = 75
        end  
    end
    
    if t.state==ENDED then  -- set x,y directions to 0
        self.dirX=0
        self.dirY=0
        
        self.leftSize = 50
        self.rightSize = 50
        self.downSize = 50
        self.upSize = 50
    end
end
--# Player
Player = class()

function Player:init(x, y, size, sprt)
    -- you can accept and set parameters here
    self.x = x
    self.y = y
    self.size = size
    self.sprite = sprt or "Planet Cute:Character Boy"
end

function Player:draw()
    -- Codea does not automatically call this method
    sprite(self.sprite, self.x, self.y, self.size)
end

function Player:touched(touch)
    -- Codea does not automatically call this method
end

