# Automatic Translation fro CZECH

# 11.03.2022
- **MULTI HOTEND**

Se viene utilizzato un hotend di tipo 2 IN 1 OUT (o xx IN 1 OUT), viene eseguita la sostituzione automatica del filamento al di fuori dell'area di stampa.
Per impostazione predefinita, il punto di sostituzione è impostato sulla posizione del finecorsa + 20 mmm. Testato anche su IDEX.
La variabile per il filamento attualmente installato viene salvata sulla scheda SD, quindi all'accensione della stampante viene impostata l'ultima configurazione.
La macro è qui: MULTI_HOTEND.cfg
Questa macro viene richiamata automaticamente dalla macro : [SETTINGS_TOOL]
Per stampanti con sistema IDEX dalla macro: [ACTIVATE_CARRIAGE]

# 14/01/2023
- **ENDLESS Spool** - bobina senza fine (utile come modalità di backup)

Se la fine del filamento viene rilevata utilizzando un sensore (interruttore o movimento), verrà eseguita la sostituzione con un estrusor che ha un filamento.
Allo stesso tempo, tutti i codici G di stampa Tx vengono indirizzati al nuovo estrusore, inclusi M104 / M109.
La regola per la sostituzione dell'estrusore è la seguente: estrusore > estrusore1 > estrusore2 > estrusore3 > ecc.
Viene utilizzato il primo estrusore che ha un filamento rilevato.
La macro è qui: [ENDLESS_SPOOL_macro.cfg]

# 26.12.2022
- **DUAL GANTRY** (doppio portale) - nuova cinematica  **`dualgantry_cartesian`** 
Si tratta di un tipo di cinematica per due teste portautensili che si muovono utilizzando due assi XY completamente indipendenti.
La macro è qui:[ACTIVATE_dual_gantry.cfg](https://github.com/DrumClock/my_config/blob/main/DUAL_GANTRY/ACTIVATE_dual_gantry.cfg)
Video di prova su: [Virtual Printer](https://youtu.be/5AJfQ59xB38)

# Informazioni

Tutte le cartelle e i file elencati sono archiviati in Raspberry qui: **`/home/pi/my_config/`**
Per visualizzare la cartella **`my_config`** nell'interfaccia **Mainsail**, è necessario creare un "soft-link" utilizzando:
**` ln -s /home/pi/my_config /home/pi/xxx/my_config `**   (xxx - directory con cfg, ad esempio klipper_config)

# Le macro sono universali per queste configurazioni

- single carriage da 2 a 4 estrusori
- dual carriage (IDEX) da 2 a 4 estrusori

Descrizione delle directory:

**Klipper non è ancora supportato: i moduli per i test sono stati forniti da Tircown**

- **`IDEX`**          - Mod M605 - DUPLICATION e MIRRORED 
- **`4EX2`**          - Estensione IDEX mod M605 per 4 extrusori (Stampa Bicolore DUPLICATION e MIRRORED)
- **`DUAL_GANTRY`**   - Doppio carrello X/Y  
#
- **`SWITCHING`**      - macro per la sostituzione dell'Estrusore o dell'Hotend  
- **`MULTI`**          - macro per HOTENDS MULTIPLI tipo 2 IN 1 OUT
#
- **`MAIN`**          - macro universali di base per 2-4 estrusori
- **`MACRO`**         - macro universali estese 
- **`MCU`**           - Configurazione HW delle schede MCU
- **`PRINTER`**       - definice druhu tiskárny pro testy
- **`TEST`**          - definizione del tipo di stampante per i test
#
- **`LCD`**           - Configurazione HW per display LCD o OLED
- **`MENU`**          - menu dello schermo per i display 
#
**nello sviluppo delle macro**
- **`P_L_R`**         - mod POWER LOSS RECOVERY  
- **`W_T`**           - mod WIPE TOWER 

# Le singole macro vengono caricate utilizzando [include ...] ad esempio:

```
### MCU configurations  ###

[include /home/pi/my_config/MCU/MCU_rumba32_.cfg]
[include /home/pi/my_config/MCU/MCU_rumba32_4ex2.cfg]
[include /home/pi/my_config/MCU/MCU_rumba32_dual_X.cfg]

### LCD configurations  ###

[include /home/pi/my_config/LCD/display_LCD12864_rumba_32.cfg]
[include /home/pi/my_config/LCD/group_klipper_logo.cfg]
[include /home/pi/my_config/LCD/group_16x4_main.cfg]

[include /home/pi/my_config/MENU/*]


### users mareo configurations  ###

[include /home/pi/my_config/MACRO/*]
[include /home/pi/my_config/W_T/*]
[include /home/pi/my_config/4EX2/*]
```

# Le variabili per le macro vengono create utilizzando [RUN_MACRO_INIT ]

avviato in [delayed_gcode _INIT] dalla configurazione della stampante dopo aver riavviato il firmware, ad es.

```
printer['gcode_macro VARIABLE'].aaa = {'printer': '4EX2'}
printer['gcode_macro VARIABLE'].active = {'z': {'restore': 4.0, 'print_offset': 0.0, 'hop': 4.0}, 'mode': 0}
printer['gcode_macro VARIABLE'].beeper = {'enable': True, 'silent': False, 'output_pin': 'beeper'}
printer['gcode_macro VARIABLE'].dual_carriage = {'parking': [-52, 298], 'movespeed': 500, 'feedrate': 30000, 'auto_exchange': True, 'sync_exchange': True}
printer['gcode_macro VARIABLE'].toolhead = {0: ['extruder', 'extruder1'], 1: ['extruder2', 'extruder3']}
printer['gcode_macro VARIABLE'].tool = {0: 'extruder', 1: 'extruder1', 2: 'extruder2', 3: 'extruder3'}
printer['gcode_macro VARIABLE'].extrude_factor = {'extruder3': 1.0, 'extruder1': 1.0, 'extruder2': 1.0, 'extruder': 1.0}
printer['gcode_macro VARIABLE'].fan = {'active': 0, 'menu': True, 'index': ['fan', 'fan1']}
printer['gcode_macro VARIABLE'].hotend_offset = {0: {'y': 0.0, 'x': 0.0, 'z': 0.0}, 1: {'y': 0.0, 'x': -10.0, 'z': 0.0}, 2: {'y': 0.0, 'x': 0.0, 'z': 0.0}, 3: {'y': 0.0, 'x': -10.0, 'z': 0.0}, 'change_T0': False}
printer['gcode_macro VARIABLE'].idex_mode = {'active': 1, 'movespeed': 500, 'feedrate': 30000, 'position': {'dupl_max': 120, 'dupl_min': 52, 'mirrored': 240}, 'carriage_offset': 0}
printer['gcode_macro VARIABLE'].neopixel = {'index': {0: 'axis_X'}, 'RGB': {0: '0,0,0'}, 'enable': True, 'menu': {'active': 0}}
printer['gcode_macro VARIABLE'].offset_temp = {'extruder3': 0, 'extruder1': 0, 'extruder2': 0, 'extruder': 0}
printer['gcode_macro VARIABLE'].restore_variable = ['active', 'hotend_offset', 'tool_change', 'tool', 'toolhead', 'switching_hotend']
printer['gcode_macro VARIABLE'].servo = {'index': {0: 'tool_1', 1: 'tool_2'}, 'enable': True, 'angle': {0: '--', 1: '--'}, 'menu': {'active': 0, 'angle_2': 135, 'angle_1': 45}}
printer['gcode_macro VARIABLE'].switching_extruder = {'enable': True, 'extruder1': 'extruder', 'extruder3': 'extruder2'}
printer['gcode_macro VARIABLE'].switching_hotend = {'enable': False, 'name': {0: 'tool_1', 1: 'tool_2'}, 'turn_off': {0: False, 1: False}, 'extruder': {'delay': 200, 'index': 0, 'angle': 45, 'off': False}, 'extruder1': {'delay': 200, 'index': 0, 'angle': 135, 'off': False}, 'extruder2': {'delay': 200, 'index': 1, 'angle': 45, 'off': False}, 'extruder3': {'delay': 200, 'index': 1, 'angle': 135, 'off': False}}
printer['gcode_macro VARIABLE'].sync_switching_tool = {'active': 'none', 'enable': True, 'T0': {0: 'extruder', 1: 'extruder2'}, 'T1': {0: 'extruder1', 1: 'extruder3'}}
printer['gcode_macro VARIABLE'].tool_change = {'delay': {'extruder': 250, 'extruder1': 250, 'extruder2': 250, 'extruder3': 250}, 'menu': 'extruder', 'enable': True, 'axis_z': {'restore': 0.0, 'hop': 3.0}, 'filament': {'load': 0.5, 'speed': 45, 'unload': 1.0}}
printer['gcode_macro VARIABLE'].tower = {'tool': {0: {'nozzle': 0.4, 'z': 0.0}, 1: {'nozzle': 0.4, 'z': 0.0}, 2: {'nozzle': 0.6, 'z': 0.0}, 3: {'nozzle': 0.8, 'z': 0.0}}, 'enable': False, 'pos': {'y': 200.0, 'x': 200.0}, 'change': True, 'gap': {'y': 40, 'x': 50}}
```



#
