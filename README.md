
<div id="top"></div>

<div align="center">
  <a href="https://github.com/renzer-xd">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a>
  <h2 align="center">คู่มือการติดตั้งและใช้งานทรัพยากร <b>RenZer Vauly</b></h2>
<hr>
</div>

# บทนำ
เพื่อให้ลูกค้าเข้าใจตรงกัน เราจะแนะนำการอ่านและดูตัวแปร Array ของโค้ดทางร้าน RenZer Developer Shop
```lua
    RZ.Vault = {
        ['สิ่งที่เก็บอยู่ในนี้ เราจะเรียกว่า Key'] = {
            สิ่งที่เก็บอยู่ในนี้ เราจะเรียกว่า Data
        }
    }
    -- Key = จะทำหน้าเป็นตัวระบุหมายเลขID Keyจะไม่สามารถใช้ชื่อซ้ำกันได้ สามารถระบุได้สองแบบคือ string,number
    -- Data = จะทำหน้าที่เก็บข้อมูลต่างๆ
```

# การติดตั้ง
1. ไปที่ โฟลเดอร์ resources ของ Serverคุณ ในกรณีที่เป็นลูกค้าใหม่ให้ทำการสร้างโฟล์เดอร์ `[RenZer] `หากเป็นลูกค้าเก่าไม่ต้องสร้างโฟลเดอร์ใหม่
2. ให้ทำการวางทรัพยากร `renzer_Vault` ลงไปในโฟล์เดอร์ `[RenZer]`
3. นำ License ที่ได้จาก [WebShop](https://renzershop.com) มาใส่ใน `Config.License.json`
4. หากคุณใช้รูปแบบฐานข้อมูล แบบ  MySql หรือ OxMysql ให้ทำการลง `renzer_vault.sql` ด้วย
5. จากนั้นให้ไปที่ server.cfg จากนั้นเพิ่ม `ensure renzer_Vault`

# การตั้งค่าทรัพยากร
ในส่วนนี้เราจะขออธิบายเฉพาะแค่หัวขอสำคัญเท่านั้นส่วนหัวข้ออื่นๆจะเขียนลงไว้ในไฟล์Configอยู่แล้ว
> ### RZ.UseDataBase 

ในส่วนนี้ ลูกค้าสามารถเลือกประเภทของการใช้งานฐานข้อมูลได้ดังนี้ `MySql,OxMysql,MongoDB` 
หากในกรณีที่ลูกค้าต้องการเก็บข้อมูลด้วย MongoDB สคริป MongoDB ของท่างต้องมี Library Sync Async
หรือสามารถโหลดได้จาก [fivem-mongodb](https://github.com/renzer-xd/fivem-mongodb)

> ### RZ.CheckDoubleAmmo

ในส่วนนี้บางกระเป๋าจะเจอปัญหาที่ดึงอาวุธออกจากตู้แล้วจะมีอาการกระสุณได้คูณ2ซึ่งในส่วนนี้จะเกิดจากตัวกระเป๋าเอง บางกระเป๋าที่แก้แล้วจะไม่เจออาการนี้หรือใครที่ใช้กระเป๋าของNcจะไม่เจออาการนี้ หากลูกค้าคนไหนเจออาการนี้ให้ปรับ `RZ.CheckDoubleAmmo = true`

> ### RZ.Vault
ในส่วนนี้ลูกค้าสามารถเพิ่มตู้เซฟได้ไม่จำกัดแต่ Key ของ `RZ.Vault` ต้องตรงกับ name ของ `RZ.SetDataBase`

> ### CheckJob
ในส่วนFunction นี้จะเป็นกำหนดให้ตู้เซฟเปิดได้แค่เฉพาะJobที่ตั้งไว้ โดยจะอิงจาก Key ของ `RZ.Vault`

# การติดตั้ง ส่วนเสริมในกระเป๋า

## esx_inventoryhud
<br>

1. ให้ทำการคัดลอกไฟล์ vault.lua โดยไปที่ (`renzer_Vault/esx_inventoryhud/vault.lua`)
2. จากนั้นให้ทำการไปที่ (`esx_inventoryhud/client`) ทำการวางไฟล์ vault.lua
3. ทำการเปิดไฟล์ `__resource.lua` หรือ `fxmanifest.lua`
4. จากนั้นมองหา 
```lua
client_scripts {
	"@es_extended/locale.lua",
	"locales/en.lua",
	"client/client.lua",
	"client/property.lua",
	"client/player.lua",
	"client/playerinventory.lua",
	"config.lua",
	'configweapons.lua'
}
```
5. ทำการว่างโค้ด `"client/vault.lua",` ไว้ข้างล่างโค้ด `"client/player.lua",` ดังตัวอย่าง
```lua
client_scripts {
	"@es_extended/locale.lua",
	"locales/en.lua",
	"client/client.lua",
	"client/property.lua",
	"client/player.lua",
    "client/vault.lua",    -- RenZer Vault
	"client/playerinventory.lua",
	"config.lua",
	'configweapons.lua'
}
```
6. จากนั้นให้ทำการไปที่ (`esx_inventoryhud/client`) ทำการเปิดไฟล์ client.lua 
7. จากนั้นมองหา function `closeInventory()` 
8. จากนั้นทำการวาง โค้ด  
```
TriggerEvent('renzer_vault:cl:CloseInventory')
```
ไว้ข้างล่างโค้ด `SetNuiFocus(false, false)` ดังตัวอย่าง
```lua
function closeInventory()
    isInInventory = false
    SendNUIMessage(
        {
            action = "hide"
        }
    )
    SetNuiFocus(false, false)
    TriggerEvent('renzer_vault:cl:CloseInventory')  -- RenZer Vault
    TriggerEvent('meeta_carinventory:setOpenMenu', false)
end
```
<br>

## nc_inventory
<br>

1. ให้ไปที่ `nc_inventory/config` จากนั้นทำการเปิดไฟล์ `config.functions.lua`
2. จากในนั้นให้มองหา function `Config.ClientOnSecondaryAction` จากนั้นให้ทำการวางโค้ด 
```lua
if secondaryName == 'vault' then
	TriggerEvent('renzer_Vault:cl:getaction',dragAction,item)
end
```
ลงไปใน function ดังตัวอย่าง
```lua
Config.ClientOnSecondaryAction = function(secondaryName, dragAction, item, job)
	if secondaryName == 'vault' then
		TriggerEvent('renzer_Vault:cl:getaction',dragAction,item)
	end
	if secondaryName == 'trunk' then
		TriggerEvent('nc_inventory_bridge:Action', secondaryName, dragAction, item)
	end
end
```
3. จากในนั้นให้มองหา function `Config.ClientOnInventoryClos` จากนั้นให้ทำการวางโค้ด 
```lua
if secondaryName == 'vault' then
	TriggerEvent('renzer_Vault:cl:CloseInventory')
end
```
ลงไปใน function ดังตัวอย่าง
```lua
Config.ClientOnInventoryClose = function(secondaryName)
	if secondaryName == 'trunk' then
		TriggerEvent('nc_inventory_bridge:CloseInventory')
	elseif secondaryName == 'vault' then
		TriggerEvent('renzer_Vault:cl:CloseInventory')
	end
end
```
# การติดตั้ง Logs เอาของเข้า/ออกเซฟ
ในส่วนนี้เราจะขอแสดงตัวอย่างของ `azael_dc-serverlogs`

> ## เอาของเข้าตู้เซฟ
1. ให้ไปที่ `renzer_Vault\config` จากนั้นเปิดไฟล์ `config.function.lua`
2. มองหา Function `RZ.ServerPutItemVault` นำโค้ดต่อไปนี้ ไปวางใน Function
```css
if type == 'item' then
    local sendToDiscord = '' .. xPlayer.name .. ' ฝาก ' .. label .. ' จำนวน ' .. ESX.Math.GroupDigits(count) .. ' ชิ้น เข้าตู้นิรภัย'
    if job == 'vault' then
		TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutItem', sendToDiscord, source, '^2') 
    else 
        if xPlayer.job.name == 'police' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutItemPolice', sendToDiscord, source, '^2')
        elseif xPlayer.job.name == 'ambulance' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutItemAmbulance', sendToDiscord, source, '^2')
        elseif xPlayer.job.name == 'mechanic' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutItemMechanic', sendToDiscord, source, '^2')
        elseif xPlayer.job.name == 'council' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutItemCouncil', sendToDiscord, source, '^2')
        end
    end
elseif type == 'account' then
    local sendToDiscord = '' .. xPlayer.name .. ' ฝาก Derty Money จำนวน $' .. ESX.Math.GroupDigits(count) .. ' เข้าตู้นิรภัย'
    if job == 'vault' then
		TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutMoney', sendToDiscord, source, '^3')
    else
        if xPlayer.job.name == 'police' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutMoneyPolice', sendToDiscord, source, '^3')
        elseif xPlayer.job.name == 'council' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutMoneyCouncil', sendToDiscord, source, '^3')
        end
    end
elseif type == 'weapon' then
    local sendToDiscord
        
    if count ~= nil and count > 1 then
        sendToDiscord = '' .. xPlayer.name .. ' ฝาก ' .. label .. ' และ กระสุน จำนวน ' .. ESX.Math.GroupDigits(count) .. ' เข้าตู้นิรภัย'
    else
        sendToDiscord = '' .. xPlayer.name .. ' ฝาก ' .. label .. ' เข้าตู้นิรภัย'
    end
    if job == 'vault' then
		TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutWeapon', sendToDiscord, source, '^3')
    else
        if xPlayer.job.name == 'police' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutWeaponPolice', sendToDiscord, source, '^2')
        elseif xPlayer.job.name == 'ambulance' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutWeaponAmbulance', sendToDiscord, source, '^2')
        elseif xPlayer.job.name == 'mechanic' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutWeaponMechanic', sendToDiscord, source, '^2')
        elseif xPlayer.job.name == 'council' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultPutWeaponCouncil', sendToDiscord, source, '^2')
        end
    end
end
```
3. อย่าลืมนำ Event Name ไปเพิ่มใน config ของ สคริป azael_dc-serverlogs ด้วย

> ## เอาของออกจากตู้เซฟ
1. ให้ไปที่ `renzer_Vault\config` จากนั้นเปิดไฟล์ `config.function.lua`
2. มองหา Function `RZ.ServerTakeItemVault` นำโค้ดต่อไปนี้ ไปวางใน Function
```css
if type == 'item' then
    local sendToDiscord = '' .. xPlayer.name .. ' นำ ' .. label .. ' จำนวน ' .. ESX.Math.GroupDigits(count) .. ' ชิ้น ออกจากตู้นิรภัย'
    if job == 'vault' then
        TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetItem', sendToDiscord, source, '^1')
    else 
        if xPlayer.job.name == 'police' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetItemJobPolice', sendToDiscord, source, '^1')
        elseif xPlayer.job.name == 'ambulance' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetItemJobAmbulance', sendToDiscord, source, '^1')
        elseif xPlayer.job.name == 'mechanic' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetItemJobMechanic', sendToDiscord, source, '^1')
        elseif xPlayer.job.name == 'council' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetItemJobCouncil', sendToDiscord, source, '^1')
        end
    end
elseif type == 'account' then
    local sendToDiscord = '' .. xPlayer.name .. ' นำ Derty Money จำนวน $' .. ESX.Math.GroupDigits(count) .. ' ออกจากตู้นิรภัย'
    if job == 'vault' then
        TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetMoney', sendToDiscord, source, '^1')
    else
        if xPlayer.job.name == 'police' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetMoneyPolice', sendToDiscord, source, '^1')
        elseif xPlayer.job.name == 'council' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetMoneyCouncil', sendToDiscord, source, '^1')
        end
    end
elseif type == 'weapon' then
    local sendToDiscord
    
    if count ~= nil and count > 1 then
        sendToDiscord = '' .. xPlayer.name .. ' ฝาก ' .. label .. ' และ กระสุน จำนวน ' .. ESX.Math.GroupDigits(count) .. ' ออกจากตู้นิรภัย'
    else
        sendToDiscord = '' .. xPlayer.name .. ' ฝาก ' .. label .. ' ออกจากตู้นิรภัย'
    end
    if job == 'vault' then
        TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetWeapon', sendToDiscord, source, '^1')
    else
        if xPlayer.job.name == 'police' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetWeaponPolice', sendToDiscord, source, '^1')
        elseif xPlayer.job.name == 'ambulance' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetWeaponAmbulance', sendToDiscord, source, '^1')
        elseif xPlayer.job.name == 'mechanic' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetWeaponMechanic', sendToDiscord, source, '^1')
        elseif xPlayer.job.name == 'council' then
            TriggerEvent('azael_dc-serverlogs:sendToDiscord', 'VaultGetWeaponCouncil', sendToDiscord, source, '^1')
        end
    end
end
```
3. อย่าลืมนำ Event Name ไปเพิ่มใน config ของ สคริป azael_dc-serverlogs ด้วย
