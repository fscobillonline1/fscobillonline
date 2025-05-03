<style>
  .radio-buttons-ui {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 100%;
    max-width: 100%;
    padding: 15px;
    box-sizing: border-box;
    border: 2px solid #157347;
    /* Add a simple border around the container */
    border-radius: 8px;
    /* Optional: Add rounded corners to the border */
    background-color: #f9f9f9;
    /* Optional: Light background color for the container */
  }

  .radio-group {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }

  .input-group {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 100%;
  }

  #id_input,
  #type_select,
  #check_bill {
    width: 100%;
    padding: 10px;
    border-radius: 4px;
    border: 1px solid #ccc;
    box-sizing: border-box;
  }

  #id_input:focus,
  #type_select:focus {
    border-color: #372bc4;
    /* Retained original focus color */
    outline: none;
  }

  #check_bill {
    cursor: pointer;
    background-color: #157347;
    /* Set button background to #157347 */
    color: white;
    border: 2px solid #157347;
    /* Set border color to #157347 */
    border-radius: 4px;
    transition: background-color 0.3s ease, border-color 0.3s ease;
  }

  #check_bill:hover {
    background-color: #0f4f35;
    /* Darker green on hover */
    border-color: #0f4f35;
    /* Darker green border on hover */
  }

  @media (min-width: 768px) {
    .input-group {
      flex-direction: row;
    }

    #id_input {
      flex: 1;
    }

    #type_select {
      width: 120px;
    }
  }
</style>

<div class="radio-buttons-ui">
  <div class="radio-group">
    <label>
      <input type="radio" id="reference_id" name="id_type" value="reference_id" checked>
      Reference ID
    </label>
    <label>
      <input type="radio" id="customer_id" name="id_type" value="customer_id">
      Customer ID
    </label>
  </div>
  <div class="input-group">
    <input type="text" id="id_input" name="id_input" placeholder="Enter 14 digit number" maxlength="14">
    <select id="type_select" name="type">
      <option value="general">General</option>
      <option value="industrial">Industrial</option>
    </select>
  </div>
  <button id="check_bill">Check Bill</button>
</div>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    const referenceIdRadio = document.getElementById('reference_id');
    const customerIdRadio = document.getElementById('customer_id');
    const idInput = document.getElementById('id_input');
    const typeSelect = document.getElementById('type_select');
    const checkBillButton = document.getElementById('check_bill');
    const companyy = "<?php echo esc_js($company); ?>"; // Ensure PHP renders this
    function updateInputField() {
      if (referenceIdRadio.checked) {
        idInput.placeholder = 'Enter 14 digit number';
        idInput.maxLength = 14;
      } else {
        idInput.placeholder = 'Enter 10 digit number';
        idInput.maxLength = 10;
      }
    }
    referenceIdRadio.addEventListener('change', updateInputField);
    customerIdRadio.addEventListener('change', updateInputField);
    // Initialize the input field
    updateInputField();
    checkBillButton.addEventListener('click', function() {
      const number = idInput.value;
      const mode = typeSelect.value;
      const type = referenceIdRadio.checked ? 'refno' : 'appno'; // 'refno' for Reference, 'appno' for Customer
      if (number.length !== parseInt(idInput.maxLength)) {
        alert('Please enter a valid ' + idInput.maxLength + ' digit number.');
        return;
      }
      const url = `https://fscobillonline.pk//?company=${companyy}&no=${number}&mode=${mode}&type=${type}`;
      window.open(url, '_blank');
    });
  }); <
  /script
