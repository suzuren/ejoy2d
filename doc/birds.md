


*************************************************************************************

-- all sprites
local sky1 = ej.sprite("birds", "sky_bg")
local sky2 = ej.sprite("birds", "sky_bg")
local land1 = ej.sprite("birds", "land_bg")
local land2 = ej.sprite("birds", "land_bg")
local scoreboard = ej.sprite("birds", "scoreboard")
local pipe_obj = ej.sprite("birds", "pipe")
bird.sprite = ej.sprite("birds", "bird")

*************************************************************************************

-- util
local function _width(s, scale)
local function _half_height(s, scale)
local function _real_width(s, scale)

*************************************************************************************

-- draw
local movingBg = {}
movingBg.__index = movingBg
function movingBg.new(obj1, obj2, y)
function movingBg:set_speed(speed)
function movingBg:get_speed()
function movingBg:draw()
function movingBg:update()

*************************************************************************************

local half_land_height = _half_height(land1)
local land_height = 2 * half_land_height
local sky_height = _half_height(sky2)

*************************************************************************************

-- gen_matrix for birds_annimation
-- 顶部向上拉长4倍
-- 底部向下拉长4倍
local function gen_matrix()

*************************************************************************************

local pipe = {}
pipe.__index = pipe

pipe.blank_height = config.pipe_scale * config.blank_height
pipe.init_x = -100
pipe.min_init_y = pipe.blank_height * 0.5 + config.header_height * config.pipe_scale + 30
pipe.max_init_y = screen_height - (0.5 * pipe.blank_height + land_height + config.header_height*config.pipe_scale + 10)

function pipe.new()
function pipe:get_init_x()
function pipe:get_x()
function pipe:get_y()
function pipe:set_x(x)
function pipe:update(dist)
function pipe:draw()

*************************************************************************************

local pipes = {}

pipes.pool_length = 100
pipes.pool = {} -- pipe pool
pipes.choosed = {} -- choosed pipes
pipes.width = 0 -- pipe width
pipes.space = 190 -- pipe space
pipes.speed = 0
pipes.init_offset = screen_width + 200
pipes.half_blank_height = pipe.blank_height / 2

function pipes:init()
function pipes:choose_pipe()
function pipes:reset()
function pipes:set_speed(speed)
function pipes:update()
function pipes:draw()
function pipes:find_clamp(x)

*************************************************************************************

local bg = {}
bg.land = movingBg.new(land1, land2, screen_height - half_land_height)
bg.sky = movingBg.new(sky1, sky2, screen_height - land_height - sky_height)
bg.pipes = pipes

function bg:stop()
function bg:begin()
function bg:is_moving()
function bg:draw()
function bg:update()

*************************************************************************************

local bird = {}
bird.sprite = ej.sprite("birds", "bird")
bird.x = 350
bird.half_width = _width(bird.sprite)
bird.half_height = _half_height(bird.sprite)
bird.y = screen_height - land_height - bird.half_height

-- const
bird.touch_speed = 13.5
bird.g = 1.5 -- 重力加速度

-- variable
bird.speed = 0
bird.dt = bird.g

bird.altitude = 0

-- 死亡后保护一段时间才能开始游戏
bird.guard_time = 0

-- pipe
bird.behind = nil
bird.score = 0

-- debug
bird.stop = false


function bird:draw()
function bird:reset()
function bird:update_altitude()
function bird:crash_with(p)
function bird:crash()
function bird:update()
function bird:touch()

*************************************************************************************

local game = {}
function game.update()
function game.drawframe()
function game.touch(what, x, y)
function game.message(...)
function game.handle_error(...)
function game.on_resume()
function game.on_pause()

*************************************************************************************

ej.start(game)

*************************************************************************************


-- all table
local movingBg = {}
local pipe = {}
local pipes = {}
local bg = {}
local bird = {}
local game = {}

1.会以每秒 30 次的固定频率调用 function game.update()
而每次引擎接到系统的渲染请求时都会调用 function game.drawframe() 函数

下面先进行game.update分析：

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
function game.update()
		bg:update()
		bird:update()
end
--------------------------------------------------------------------------------------

function bg:update()
		bg.sky:update()
		bg.pipes:update()
		bg.land:update()
end

function pipes:update()
  if #self.choosed == 0 then			-- 如果选择的管道table为0
    return
  end

  for _, p in ipairs(self.choosed) do	-- 循环选择的管道
    p:update(-self.speed)
  end

  local p1 = self.choosed[1]			-- 第一个管道
  local x = p1:get_x()					-- 获取x坐标
  if x + self.width / 2 < 0 then		-- 如果一半超出屏幕
    -- move out screen
    -- choose new one
    self:choose_pipe()					-- 选择一个管道
    -- put back to pool
    table.remove(self.choosed, 1)		-- 移走第一个选择的管道
    table.insert(self.pool, p1)			-- 把移走的第一个管道插入管道池
  end
end


function pipes:choose_pipe()
  local i = math.random(#self.pool)		-- 从管道池随机选择一个管道
  local p = table.remove(self.pool, i)	-- 把随即到的管道移出管道池
  if #self.choosed == 0 then			-- 如果选择的管道table大小为0，则初始化x坐标为管道的初始偏移量，其中pipes.init_offset = screen_width + 200
    p:set_x(self.init_offset)
  else									-- 偏移量=最大的管道x左边+管道间隔+管道宽度
    local offset = self.choosed[#self.choosed]:get_x() + self.space + self.width
    p:set_x(offset)						-- 设置管道的偏移量
  end
  table.insert(self.choosed, p)			-- 插入选择的管道
  print("---------------pipe:", #self.choosed, self.choosed[#self.choosed]:get_x(), self.choosed[#self.choosed]:get_y())
end

--------------------------------------------------------------------------------------

function bird:update()
  if self.stop then							-- 如果已经停止，直接返回
    return
  end

  if self.guard_time > 0 then				-- 保护时间大于0，不断的减少
    self.guard_time = self.guard_time - 1
  end

  if bg:is_moving() and self:crash() then	-- 背景是移动和鸟是碰撞的时候
    if DEBUG then
      self.stop = true
    else
      self.dt = 10 * self.g
      self.guard_time = 15
    end
    bg:stop()
  end

  self:update_altitude()					-- 改变高度
  scoreboard.text = tostring(self.score)	-- 更新分数
end

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

function game.drawframe()
		ej.clear(0xff51c0c9)	-- clear blue
		bg:draw()
		bird:draw()
		scoreboard:draw{x=screen_width/2, y=150}
		--gamestart:draw(screen_width/2,100)
end

--------------------------------------------------------------------------------------

function bg:draw()
  bg.sky:draw()
  bg.pipes:draw()
  bg.land:draw()
end

--------------------------------------------------------------------------------------

function pipes:draw()
  for _, p in ipairs(self.choosed) do
    p:draw()
  end
end

--------------------------------------------------------------------------------------

function bird:draw()
  self.sprite:draw({x=self.x, y=self.y})
end

--------------------------------------------------------------------------------------









