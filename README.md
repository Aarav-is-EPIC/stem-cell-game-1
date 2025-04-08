<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stem Cell Simulation Lab</title>
    <style>
        body {
            font-family: sans-serif;
            display: flex;
            justify-content: center;
            align-items: flex-start; /* Align items to the top */
            padding-top: 30px;
            background-color: #f0f0f0;
            min-height: 100vh;
            gap: 30px; /* Space between dish and controls */
        }

        .simulation-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            text-align: center; /* Center text inside this container */
        }

        h1, h2 {
            color: #333;
            margin-bottom: 15px;
        }

        .disclaimer {
            font-size: 0.8em;
            color: #e74c3c; /* Red color for emphasis */
            margin-bottom: 20px;
            border: 1px solid #e74c3c;
            padding: 8px;
            border-radius: 4px;
            background-color: #fdd; /* Light red background */
            max-width: 500px;
            text-align: left; /* Align text left inside the box */
        }

        #petri-dish {
            width: 250px;
            height: 250px;
            border: 3px solid #55aacc;
            border-radius: 50%;
            background-color: #e0f7ff;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative; /* Needed for absolute positioning of the cell */
            overflow: hidden; /* Keep cell elements inside */
            margin-bottom: 20px;
        }

        #cell {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background-color: #f1c40f; /* Stem cell color */
            border: 2px solid #c0392b;
            position: absolute; /* Center regardless of content */
            transition: all 0.6s ease-in-out; /* Smooth transitions */
            display: flex; /* For potential internal structures */
            justify-content: center;
            align-items: center;
        }

        /* Specialized Cell Styles */
        .cell-neuron {
            background-color: #3498db; /* Neuron blue */
            border-radius: 30% 70% 70% 30% / 30% 30% 70% 70%; /* Irregular shape */
            width: 70px;
            height: 50px;
        }
         /* Basic attempt at axon/dendrite visuals */
        .cell-neuron::before, .cell-neuron::after {
             content: '';
             position: absolute;
             background-color: #2980b9;
             height: 5px;
             border-radius: 3px;
         }
         .cell-neuron::before {
             width: 40px;
             left: -35px; /* Extend left */
             top: 45%;
         }
          .cell-neuron::after {
             width: 25px;
             right: -20px; /* Extend right */
             top: 30%;
             transform: rotate(20deg);
         }

        .cell-muscle {
            background-color: #e74c3c; /* Muscle red */
            width: 120px;
            height: 40px;
            border-radius: 20px; /* Elongated pill shape */
             border: 2px solid #c0392b;
        }
        /* Basic striations */
         .cell-muscle::before {
             content: '';
             position: absolute;
             width: 90%;
             height: 50%;
             border-top: 2px dashed #c0392b;
             border-bottom: 2px dashed #c0392b;
             top: 25%;
             left: 5%;
         }


        .cell-skin {
            background-color: #f39c12; /* Skin orange/beige */
            width: 70px;
            height: 70px;
            border-radius: 15%; /* Squarish/Polygonal */
            border: 2px solid #d35400;
        }


        /* Intermediate State Example (optional) */
        .cell-neuroblast {
             background-color: #8eceed; /* Lighter blue */
             border-radius: 40% 60% 50% 50%;
        }

        .controls-container {
            display: flex;
            flex-direction: column;
            align-items: center;
             background-color: #fff;
             padding: 20px;
             border-radius: 8px;
             box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        .factor-buttons {
            display: grid;
            grid-template-columns: repeat(2, 1fr); /* 2 columns */
            gap: 10px;
            margin-bottom: 20px;
        }

        .factor-btn {
            padding: 10px 15px;
            font-size: 1em;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            background-color: #2ecc71; /* Green factors */
            color: white;
            transition: background-color 0.3s ease;
        }
        .factor-btn:hover {
            background-color: #27ae60;
        }
        .factor-btn:active {
             background-color: #1e8449;
        }


        #status-display {
            margin-top: 15px;
            padding: 15px;
            border: 1px solid #bdc3c7;
            border-radius: 5px;
            background-color: #ecf0f1;
             min-width: 250px; /* Ensure minimum width */
             text-align: left; /* Align text left inside status box */
        }
         #status-display h3 {
             margin-top: 0;
             color: #2c3e50;
         }
         #status-display p {
             margin-bottom: 0;
             color: #34495e;
             font-size: 0.9em;
         }

         #reset-button {
              padding: 10px 20px;
              font-size: 1em;
              cursor: pointer;
              border: none;
              border-radius: 5px;
              background-color: #e67e22; /* Orange for reset */
              color: white;
              transition: background-color 0.3s ease;
              margin-top: 20px;
         }
         #reset-button:hover {
             background-color: #d35400;
         }

    </style>
</head>
<body>

    <div class="simulation-container">
        <h1>Stem Cell Simulation</h1>
         <div class="disclaimer">
            <strong>Disclaimer:</strong> This is a highly simplified educational simulation ONLY. It does not accurately represent the complex biological processes of stem cell differentiation. It is for illustrative and entertainment purposes.
        </div>
        <div id="petri-dish">
            <div id="cell" class="cell-stem"></div> <!-- Start as stem cell -->
        </div>
         <button id="reset-button">Reset to Stem Cell</button>
    </div>

    <div class="controls-container">
         <h2>Add Differentiation Factors</h2>
        <div class="factor-buttons">
            <button class="factor-btn" data-factor="NeuroGrow">NeuroGrow (NGF-like)</button>
            <button class="factor-btn" data-factor="AxonSignal">Axon Signal</button>
            <button class="factor-btn" data-factor="MyoBlastInit">MyoBlastInit (Muscle Start)</button>
            <button class="factor-btn" data-factor="MuscleMatrix">Muscle Matrix</button>
             <button class="factor-btn" data-factor="EctoFactor">EctoFactor (Skin)</button>
            <button class="factor-btn" data-factor="KeratinSignal">Keratin Signal</button>
            <!-- Add more buttons here -->
        </div>

         <div id="status-display">
             <h3 id="cell-type-name">Stem Cell</h3>
             <p id="cell-description">A pluripotent cell. Ready to differentiate!</p>
        </div>
    </div>


    <script>
        const cellElement = document.getElementById('cell');
        const cellTypeNameElement = document.getElementById('cell-type-name');
        const cellDescriptionElement = document.getElementById('cell-description');
        const factorButtons = document.querySelectorAll('.factor-btn');
        const resetButton = document.getElementById('reset-button');

        let currentCellState = 'stem'; // Initial state

        // Define cell types and their info
        const cellInfo = {
            'stem': { name: 'Stem Cell', description: 'A pluripotent cell. Ready to differentiate!', cssClass: 'cell-stem' },
             'neuroblast': { name: 'Neuroblast', description: 'Commited to neural lineage. Needs specific signals.', cssClass: 'cell-neuroblast' },
            'neuron': { name: 'Neuron', description: 'Specialized nerve cell. Transmits signals.', cssClass: 'cell-neuron' },
             'myoblast': { name: 'Myoblast', description: 'Commited muscle precursor. Needs maturation signals.', cssClass: 'cell-myoblast' }, // Need CSS for this if used visually
            'muscle': { name: 'Muscle Cell', description: 'Contractile cell for movement.', cssClass: 'cell-muscle' },
             'keratinocyte_precursor': { name: 'Keratinocyte Precursor', description: 'Commited skin cell precursor.', cssClass: 'cell-keratinocyte-precursor' }, // Need CSS
            'skin': { name: 'Skin Cell (Keratinocyte)', description: 'Forms protective skin layer.', cssClass: 'cell-skin' }
        };

        // Define simplified differentiation rules: [currentState]: { factor: nextState }
        const differentiationRules = {
            'stem': {
                'NeuroGrow': 'neuroblast',
                'MyoBlastInit': 'myoblast',
                 'EctoFactor': 'keratinocyte_precursor'
            },
             'neuroblast': {
                 'AxonSignal': 'neuron'
                // Other factors might block or have no effect
            },
             'myoblast': {
                'MuscleMatrix': 'muscle'
            },
             'keratinocyte_precursor': {
                 'KeratinSignal': 'skin'
             },
            // Terminal states (cannot differentiate further)
            'neuron': {},
            'muscle': {},
            'skin': {}
        };

        function updateDisplay() {
            const info = cellInfo[currentCellState];
            if (info) {
                cellTypeNameElement.textContent = info.name;
                cellDescriptionElement.textContent = info.description;

                // Update cell visual class - remove all specific classes first
                cellElement.className = 'cell'; // Reset to base class
                 if (info.cssClass) {
                      cellElement.classList.add(info.cssClass);
                 } else {
                      // Default style if no specific class (might happen for intermediate states without unique visuals)
                     cellElement.classList.add('cell-stem'); // Fallback visual
                     console.warn("No specific CSS class defined for state:", currentCellState);
                 }

            } else {
                 // Fallback if state somehow becomes unknown
                cellTypeNameElement.textContent = 'Unknown State';
                 cellDescriptionElement.textContent = 'Something went wrong in the simulation.';
                  cellElement.className = 'cell'; // Reset visual
                  cellElement.classList.add('cell-stem'); // Fallback visual
            }
        }

        function applyFactor(factor) {
             const rulesForCurrentState = differentiationRules[currentCellState];
            let message = `Factor '${factor}' applied. `;

            if (rulesForCurrentState && rulesForCurrentState[factor]) {
                // Valid differentiation step
                const nextState = rulesForCurrentState[factor];
                 currentCellState = nextState;
                message += ` Cell is now differentiating into a ${cellInfo[currentCellState]?.name || nextState}.`;
                 updateDisplay();
             } else if (!rulesForCurrentState || Object.keys(rulesForCurrentState).length === 0) {
                 // Terminal state or unknown state
                 message += `This cell is terminally differentiated or in an unknown state and cannot change further with this factor.`;
                  cellDescriptionElement.textContent = message; // Update description with feedback
            } else {
                 // Factor has no effect in this state
                 message += `It has no effect on the current ${cellInfo[currentCellState]?.name || currentCellState}.`;
                  cellDescriptionElement.textContent = message; // Update description with feedback
            }
            console.log(message); // Log to console for debugging
        }

         function resetSimulation() {
              currentCellState = 'stem';
              updateDisplay();
              cellDescriptionElement.textContent = cellInfo.stem.description + " Simulation Reset.";
              console.log("Simulation Reset");
         }


        // Add event listeners to buttons
        factorButtons.forEach(button => {
            button.addEventListener('click', () => {
                const factor = button.getAttribute('data-factor');
                applyFactor(factor);
            });
        });

         // Add event listener for reset button
         resetButton.addEventListener('click', resetSimulation);

        // Initial setup on page load
        updateDisplay();
        console.log("Simulation initialized. Current state:", currentCellState);

    </script>

</body>
</html>
