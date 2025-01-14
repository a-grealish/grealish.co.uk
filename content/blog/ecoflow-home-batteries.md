---
author: Ashley Grealish
category:
- sustainability
date: "2025-01-10"
description: "Using small, portable batteries to save money on your home enrgy bill"
tags: []
thumbnail: /img/todo
title: "TODO"
---

Lot’s of people have been buying portable battery systems, like those from EcoFlow or their competitors. These are really useful for taking power with you when your camping, or doing work away from the grid. But, they also often sit unused in the house for long periods. 

What if they could be used to save money on your energy bills or reduce your carbon emissions whilst unused at home?

The idea is simple, store energy in the battery when renewables are plentiful, and energy is cheap. And, use that stored energy to power your home, when you need them.

In this article, I will propose a number of ways you can achieve this. From using the battery to power a single device, to powering your whole home.

This technique can work with any portable batteries. But it most impactful when above 1 kWh or above. The EcoFlow Delta series is great, it ranges from 1-6 kWh (or even up to 25 kWh for the Pro model). Let’s use that as a starting example

## Powering a single device

The EcoFlow product range has AC outputs built in, which can cover almost any home appliance that you plug in to it. You can also plug in multiple AC devices. For example, if you have a desk with a monitor, laptop charger and a lamp, you can power all of these from one EcoFlow. This will typically be <1kWh of energy, can be easily powered all day from 1 charge.

The downside of this approach is maximising your use of the battery. For optimal savings, you want to use most of the capacity of the battery each day. You need to experiment, or do some maths, to work out which device makes the best use of the battery you have.

To make this work, you’ll need:
- An EcoFlow Battery
- A smart-plug to schedule charging

```mermaid
graph LR
  SP[Smart Plug]-->|Charging Input|EFD[EcoFlow Delta]
  EFD-->|AC Output|Device
```

![Using an EcoFlow Delta 2 to power my desk](/img/ecoflow/delta2-under-desk.jpg)


## Powering your full home

To power more than one device in your home, you can add a micro-inverter to your battery. EcoFlow's micro-inverter is called PowerFlow. With this, the battery can feed power to all your appliances via your home's existing wiring. This can be used to send power to your lights, TV, and partly power your Electric Hob and Oven. The maximum output of the PowerStream is 800W, but any power required by your home above this will be pulled from the grid, as before.

You’ll need:
- An EcoFlow Battery
- An EcoFlow PowerSteam Inverter
- A cable to connect the above two
- 2 x Smart Plugs to schedule charge and discharge times
- A Shelly 3EM to measure your consumption (or EcoFlow Smart Plugs)

```mermaid
graph LR
  SP[Smart Plug]-->|Charging Input|EFD[EcoFlow Delta]
  EFD --> PowerStream
  3EM[Shelly 3EM]-.->PowerStream
  PowerStream-->|AC Output|SmartPlug
  
```

![EcoFlow Delta 2 with a Powerstream micro-inverter, with two smartplugs](/img/ecoflow/delta2-with-powerstream.jpg)

## How much could I save?
Calculate the potential savings for yourself. The biggest factor in your potential savings is the energy tariff or rate plan. 

<style>
    .calculator {
        width: 100%;
        padding: 20px;
        border: 2px solid black;
        box-sizing: border-box;
    }
    .row {
        display: flex;
        gap: 20px;
        margin-bottom: 20px;
        --bs-gutter-x: 0;
    }
    .input-group {
        background-color: #eee;
        border: 2px solid black;
        padding: 10px;
        flex: 1;
    }
    .battery-select-group {
        display: flex;
        gap: 20px;
        align-items: flex-end;
    }
    .checkbox-group {
        display: flex;
        align-items: center;
        gap: 10px;
        background-color: #eee;
        border: 2px solid black;
        padding: 10px;
        height: fit-content;
    }
    .checkbox-group label {
        margin: 0;
    }
    label {
        display: block;
        margin-bottom: 5px;
        font-weight: bold;
    }
    input, select {
        width: calc(100% - 20px);
        padding: 8px;
        border: 2px solid black;
        font-family: inherit;
        font-size: 16px;
        background-color: #ffffff;
        box-sizing: border-box;
    }
    input[type="checkbox"] {
        width: auto;
        margin: 0;
        cursor: pointer;
    }
    input:focus, select:focus {
        outline: none;
        background-color: #f2ca00;
    }
    #manual-capacity {
        display: none;
    }
    .result {
        padding: 15px;
        background-color: #f2ca00;
        border: 2px solid black;
        color: black;
        font-size: 18px;
        font-weight: bold;
        text-transform: uppercase;
    }
    #savings {
    	font-size: 2rem;
	}
</style>

<div class="calculator">
    <div class="row">
        <div class="input-group">
            <label for="model">EcoFlow Model:</label>
            <select id="model"></select>
        </div>
        <div class="checkbox-group">
            <label for="powerstream">PowerStream Installed:</label>
            <input type="checkbox" id="powerstream">
        </div>
    </div>
    <div class="row">
        <div class="input-group" id="manual-capacity">
            <label for="capacity">Battery Capacity (kWh):</label>
            <input type="number" id="capacity" value="1" step="0.1">
        </div>
    </div>
    <div class="row">
        <div class="input-group">
            <label for="peakCost">Peak Energy Cost (£/kWh):</label>
            <input type="number" id="peakCost" value="0.35" step="0.01">
        </div>
        <div class="input-group">
            <label for="offPeakCost">Off-Peak Energy Cost (£/kWh):</label>
            <input type="number" id="offPeakCost" value="0.15" step="0.01">
        </div>
    </div>
    <div class="result">
        Estimated Yearly Savings: £<span id="savings">0</span>
    </div>
</div>

<script>
    // Single source of truth for all EcoFlow product configurations
    const ecoflowConfigs = {
        'delta2': {
            name: 'DELTA 2',
            capacity: 1.024,
            powerStreamCompatible: true
        },
        'delta2_extra': {
            name: 'DELTA 2 + Extra Battery',
            capacity: 2.048,
            powerStreamCompatible: false
        },
        'delta2_max': {
            name: 'DELTA 2 Max',
            capacity: 2.016,
            powerStreamCompatible: true
        },
        'delta2_max_1extra': {
            name: 'DELTA 2 Max + 1 Extra Battery',
            capacity: 4.032,
            powerStreamCompatible: true
        },
        'delta2_max_2extra': {
            name: 'DELTA 2 Max + 2 Extra Batteries',
            capacity: 6.048,
            powerStreamCompatible: false
        },
        'delta3': {
            name: 'DELTA 3',
            capacity: 1.0,
            powerStreamCompatible: true
        },
        'delta3_extra': {
            name: 'DELTA 3 + Extra Battery',
            capacity: 2.0,
            powerStreamCompatible: false
        },
        'other': {
            name: 'Other',
            capacity: null,
            powerStreamCompatible: true
        }
    };

    // Generate HTML for the model selector
    function generateModelSelect() {
        const select = document.getElementById('model');
        const powerStreamInstalled = document.getElementById('powerstream').checked;
        select.innerHTML = ''; // Clear existing options

        Object.entries(ecoflowConfigs).forEach(([key, config]) => {
            // Skip Delta 2 options if PowerStream is installed
            if (powerStreamInstalled && !config.powerStreamCompatible) {
                return;
            }

            if (key === 'other') {
                select.innerHTML += `<option value="${key}">${config.name}</option>`;
            } else {
                select.innerHTML += `<option value="${key}">${config.name} (${config.capacity}kWh)</option>`;
            }
        });

        // If current selection is not compatible with PowerStream, select first compatible option
        if (powerStreamInstalled && !ecoflowConfigs[select.value]?.powerStreamCompatible) {
            select.value = Object.keys(ecoflowConfigs).find(key => 
                ecoflowConfigs[key].powerStreamCompatible
            );
        }
    }

    // Get DOM elements
    const modelSelect = document.getElementById('model');
    const powerStreamCheckbox = document.getElementById('powerstream');
    const manualCapacity = document.getElementById('manual-capacity');
    const capacityInput = document.getElementById('capacity');
    const peakCostInput = document.getElementById('peakCost');
    const offPeakCostInput = document.getElementById('offPeakCost');
    const savingsElement = document.getElementById('savings');

    // Constants
    const CHARGE_EFFICIENCY = 0.95;
    const DISCHARGE_EFFICIENCY = 0.95;
    const DAYS_PER_YEAR = 365;

    // Initialize selector
    generateModelSelect();

    // Calculate function
    function calculateSavings() {
        let capacity;
        if (modelSelect.value === 'other') {
            capacity = parseFloat(capacityInput.value);
        } else {
            capacity = ecoflowConfigs[modelSelect.value].capacity;
        }

        const peakCost = parseFloat(peakCostInput.value);
        const offPeakCost = parseFloat(offPeakCostInput.value);

        const yearlySavings = (
            (capacity * DISCHARGE_EFFICIENCY * peakCost)
            - ((capacity / CHARGE_EFFICIENCY) * offPeakCost)
        ) * DAYS_PER_YEAR;

        savingsElement.textContent = yearlySavings.toFixed(2);
    }

    // Event listeners
    modelSelect.addEventListener('change', function() {
        manualCapacity.style.display = this.value === 'other' ? 'block' : 'none';
        calculateSavings();
    });

    powerStreamCheckbox.addEventListener('change', function() {
        generateModelSelect();
        calculateSavings();
    });

    [capacityInput, peakCostInput, offPeakCostInput].forEach(input => {
        input.addEventListener('input', calculateSavings);
    });

    // Initial calculation
    calculateSavings();
</script>

<br>

## Which smart plugs should I use?

All of these use-cases require a smart plug to schedule the charging, and discharging of the battery. This can be set on simple timers. If you know you are on an energy tariff which costs less overnight, you can program this into the timers.

But, if you want to maximise your carbon and costs savings, you’ll want to integrate more deeply with the grid and your energy supplier. [Windfall Energy](https://www.windfallenergy.com) plugs enable this. They will charge you battery when energy is greenest and cheapest, and divert the power back to your home at peak times.


