# Guess-my-number
An awesome game developed for Home Assistant, where HA can guess any number of your choice between 0-99 by showing some number cards to you. For more details on this project, do watch my video number #50 on vccground youtube channel https://www.youtube.com/@the_vccground

## How to Setup ?

You need to create 10 helpers, 3 sensors, 2 Scripts and 1 Automation.

Step 1: Download the ZIP. In your Home Assistant config folder, create a new folder named www (if you havn't already) and paste all eight pictures of the number cards in it.

Step 2: Go to Settings/Devices&Services/Helpers and create eight different toggle helpers (Card A to Card H), Another toggle helper with name (Start) and another with name (Show my Number).

Step 3: Open your configuration.yaml file and paste the following three sensor configuration under the Sensor heading as shown below.
```
sensor:
  - platform: template
    sensors:  
      number1:
        value_template: >-
          {% if is_state('input_boolean.card_a', 'off') and is_state('input_boolean.card_b', 'off') and is_state('input_boolean.card_c', 'off') and is_state('input_boolean.card_d', 'off') %}
            0
          {% elif is_state('input_boolean.card_a', 'on') and is_state('input_boolean.card_b', 'off') and is_state('input_boolean.card_c', 'off') and is_state('input_boolean.card_d', 'off') %}
            8
          {% elif is_state('input_boolean.card_a', 'off') and is_state('input_boolean.card_b', 'on') and is_state('input_boolean.card_c', 'off') and is_state('input_boolean.card_d', 'off') %}
            4
          {% elif is_state('input_boolean.card_a', 'off') and is_state('input_boolean.card_b', 'off') and is_state('input_boolean.card_c', 'on') and is_state('input_boolean.card_d', 'off') %}
            2
          {% elif is_state('input_boolean.card_a', 'off') and is_state('input_boolean.card_b', 'off') and is_state('input_boolean.card_c', 'off') and is_state('input_boolean.card_d', 'on') %}
            1
          {% elif is_state('input_boolean.card_a', 'off') and is_state('input_boolean.card_b', 'off') and is_state('input_boolean.card_c', 'on') and is_state('input_boolean.card_d', 'on') %}
            3
          {% elif is_state('input_boolean.card_a', 'off') and is_state('input_boolean.card_b', 'on') and is_state('input_boolean.card_c', 'off') and is_state('input_boolean.card_d', 'on') %}
            5
          {% elif is_state('input_boolean.card_a', 'off') and is_state('input_boolean.card_b', 'on') and is_state('input_boolean.card_c', 'on') and is_state('input_boolean.card_d', 'on') %}
            7
          {% elif is_state('input_boolean.card_a', 'on') and is_state('input_boolean.card_b', 'off') and is_state('input_boolean.card_c', 'off') and is_state('input_boolean.card_d', 'on') %}
            9
          {% elif is_state('input_boolean.card_a', 'off') and is_state('input_boolean.card_b', 'on') and is_state('input_boolean.card_c', 'on') and is_state('input_boolean.card_d', 'off') %}
            6            
          {% else %}
            error
          {% endif %}      
      number2:
        value_template: >-
          {% if is_state('input_boolean.card_e', 'off') and is_state('input_boolean.card_f', 'off') and is_state('input_boolean.card_g', 'off') and is_state('input_boolean.card_h', 'off') %}
            0
          {% elif is_state('input_boolean.card_e', 'on') and is_state('input_boolean.card_f', 'off') and is_state('input_boolean.card_g', 'off') and is_state('input_boolean.card_h', 'off') %}
            8
          {% elif is_state('input_boolean.card_e', 'off') and is_state('input_boolean.card_f', 'on') and is_state('input_boolean.card_g', 'off') and is_state('input_boolean.card_h', 'off') %}
            4
          {% elif is_state('input_boolean.card_e', 'off') and is_state('input_boolean.card_f', 'off') and is_state('input_boolean.card_g', 'on') and is_state('input_boolean.card_h', 'off') %}
            2
          {% elif is_state('input_boolean.card_e', 'off') and is_state('input_boolean.card_f', 'off') and is_state('input_boolean.card_g', 'off') and is_state('input_boolean.card_h', 'on') %}
            1
          {% elif is_state('input_boolean.card_e', 'off') and is_state('input_boolean.card_f', 'off') and is_state('input_boolean.card_g', 'on') and is_state('input_boolean.card_h', 'on') %}
            3
          {% elif is_state('input_boolean.card_e', 'off') and is_state('input_boolean.card_f', 'on') and is_state('input_boolean.card_g', 'off') and is_state('input_boolean.card_h', 'on') %}
            5
          {% elif is_state('input_boolean.card_e', 'off') and is_state('input_boolean.card_f', 'on') and is_state('input_boolean.card_g', 'on') and is_state('input_boolean.card_h', 'on') %}
            7
          {% elif is_state('input_boolean.card_e', 'on') and is_state('input_boolean.card_f', 'off') and is_state('input_boolean.card_g', 'off') and is_state('input_boolean.card_h', 'on') %}
            9
          {% elif is_state('input_boolean.card_e', 'off') and is_state('input_boolean.card_f', 'on') and is_state('input_boolean.card_g', 'on') and is_state('input_boolean.card_h', 'off') %}
            6            
          {% else %}
            error
          {% endif %} 
      final_number:
        value_template: >-
          {{ states.sensor.number2.state }}{{ states.sensor.number1.state }}
```
Step 4: Go to Scripts & Click 'Add Script' at the bottom of the page. Click 3 dots on the upper right corner and select 'Edit in YAML'. Copy the following code and paste the same in the yaml editor and click save to save the Reset Cards Script.
```
alias: Reset Cards
sequence:
  - service: input_boolean.turn_off
    data: {}
    target:
      entity_id:
        - input_boolean.card_a
        - input_boolean.card_b
        - input_boolean.card_c
        - input_boolean.card_d
        - input_boolean.card_e
        - input_boolean.card_f
        - input_boolean.card_g
        - input_boolean.card_h
        - input_boolean.guess_my_number
mode: single
icon: mdi:cards
```
Step 5 (optional): Follow the same procedure to create and save the Announce my number Script.
```
alias: Announce my Number
sequence:
  - service: notify.alexa_media
    data_template:
      target: media_player.<<name_of_your_media_player_here>>*
      data:
        type: tts
      message: >-
        {{ ["your number is", "this time your number is", "I guess, its", "Very
        easy, its", "It seems to be", "I know, its"] | random }} {{
        states.sensor.final_number.state }}.
mode: single
icon: mdi:bullhorn-outline
```
*Remember to write the name of your own media player in target field. Refer to video #50 on my YouTube channel if having any doubt in this step.

Step 6: Similarly, Go to Automations & Create a new automation. Click 3 dots on the upper right corner and select 'Edit in YAML'. Copy the following code and paste the same in the yaml editor and click save to save the Automation.
```
alias: Reset Cards at start
description: ""
trigger:
  - platform: state
    entity_id:
      - input_boolean.start
    from: "on"
    to: "off"
condition: []
action:
  - service: script.reset_cards
    data: {}
mode: single
```

With these six steps, the Backend setup is completed. Now we need to create a dashboard for the frontend.

For that, in your lovelace dashboard page, Click 3 dots on the upper right corner and select 'Edit Dashboard'. Click Add Card at the bottom, select any card here and select, Show code editor, at the bottom. Delete any existing content here. Copy the following code and paste the same in the yaml editor and click save to save the New Card.
```
type: vertical-stack
title: Play Guess My Number...
cards:
  - type: horizontal-stack
    cards:
      - show_name: true
        show_icon: true
        type: button
        tap_action:
          action: call-service
          service: input_boolean.toggle
          target:
            entity_id: input_boolean.start
          data: {}
        entity: input_boolean.start
      - show_name: true
        show_icon: true
        type: button
        tap_action:
          action: call-service
          service: script.reset_cards
          target: {}
          data: {}
        entity: script.reset_cards
  - type: conditional
    conditions:
      - entity: input_boolean.start
        state: 'on'
    card:
      type: vertical-stack
      title: Tap the cards with your number...
      cards:
        - show_state: true
          show_name: true
          camera_view: auto
          type: picture-entity
          entity: input_boolean.card_a
          image: http://10.0.0.150:8123/local/A.jpg
          tap_action:
            action: toggle
        - show_state: true
          show_name: true
          camera_view: auto
          type: picture-entity
          entity: input_boolean.card_b
          image: http://10.0.0.150:8123/local/B.jpg
          tap_action:
            action: toggle
        - show_state: true
          show_name: true
          camera_view: auto
          type: picture-entity
          entity: input_boolean.card_c
          image: http://10.0.0.150:8123/local/C.jpg
          tap_action:
            action: toggle
        - show_state: true
          show_name: true
          camera_view: auto
          type: picture-entity
          entity: input_boolean.card_d
          image: http://10.0.0.150:8123/local/D.jpg
          tap_action:
            action: toggle
        - show_state: true
          show_name: true
          camera_view: auto
          type: picture-entity
          entity: input_boolean.card_e
          image: http://10.0.0.150:8123/local/E.jpg
          tap_action:
            action: toggle
        - show_state: true
          show_name: true
          camera_view: auto
          type: picture-entity
          entity: input_boolean.card_f
          image: http://10.0.0.150:8123/local/F.jpg
          tap_action:
            action: toggle
        - show_state: true
          show_name: true
          camera_view: auto
          type: picture-entity
          entity: input_boolean.card_g
          image: http://10.0.0.150:8123/local/G.jpg
          tap_action:
            action: toggle
        - show_state: true
          show_name: true
          camera_view: auto
          type: picture-entity
          entity: input_boolean.card_h
          image: http://10.0.0.150:8123/local/H.jpg
          tap_action:
            action: toggle
        - type: horizontal-stack
          cards:
            - show_name: true
              show_icon: true
              type: button
              tap_action:
                action: call-service
                service: input_boolean.toggle
                target:
                  entity_id: input_boolean.show_my_number
              entity: input_boolean.show_my_number
            - show_name: true
              show_icon: true
              type: button
              entity: script.announce_my_number
              show_state: false
              tap_action:
                action: call-service
                service: script.announce_my_number
                service_data: {}
                target: {}
        - type: conditional
          conditions:
            - entity: input_boolean.show_my_number
              state: 'on'
          card:
            type: vertical-stack
            cards:
              - type: entity
                entity: sensor.final_number
                name: Your Number

```
*Remember to replace 10.0.0.150 with your own HA IP in all the eight cards configuration in above code.

Thats it !!! Everything is well explained on "vccground" YouTube Channel https://www.youtube.com/@the_vccground , Please refer to Video #50 for deep understanding.

Join "vccground" telegram channel https://t.me/vccground for any discussion on all my project.

