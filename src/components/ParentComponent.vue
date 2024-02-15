<!-- ExampleParent.vue -->

  <template>
    <div>
      <div class="p-2 max-w-sm mx-auto bg-white flex items-center justify-between	space-x-4 border-spacing-2">
        <button @click="showSingleSelect=true" class=" p-3" :class="showSingleSelect ? 'bg-slate-700 text-white' : 'bg-white'">SINGLE SELECT</button>
        <button @click="showSingleSelect=false" class="p-3" :class="!showSingleSelect ? 'bg-slate-700 text-white' : 'bg-white'">MULTI SELECT</button>
      </div>
      
      <Dropdown 
        :items="getdropdownItems" 
        placeholder="Select Single Country" 
        @selectedItems="handleSelectedItems"
        @searchItem="handleSearchItems"
        :resetDropdown="resetSingleDropdown"
        v-if="showSingleSelect"
      />
      <Dropdown 
        :items="getdropdownItems" 
        placeholder="Select Multiple Countries" 
        :multiselect="true"
        :resetDropdown="resetMultiDropdown"
        @selectedItems="handleSelectedItems"
        @searchItem="handleSearchItems"
        v-else
      />


      <div class="m-5 hidden">
        Developer Note :
        <p> 1. Same Dropdown can be Used as single select / Multi select
               Pass multiselect as true for enabling multi select dropdown option
        
            2. For resetting any value in the dropdown:
                * Single select - pass resetDropdown as Object
                * Multi Select - pass resetDropdown as Array

            3. List values - pass as items
               Format - [
                          {"title": "", "value": ""}
                        ]

            4. Search functionality also added. : Event Used - searchItem
            5. Multi sealect values sends to parent component as array and single select, pass it as object.
               Event Used - selectedItems
        </p>
      </div>
    </div>
  </template>
  
  <script>
  import Dropdown from './MultiSelectDropdown.vue';
  import { ref, computed } from 'vue';
  export default {
    components: {
      Dropdown,
    },
    setup () {
      // Data
      const dropdownItems = [
            {"title": "Afghanistan", "value": "AF"}, 
            {"title": "land Islands", "value": "AX"}, 
            {"title": "Albania", "value": "AL"}, 
            {"title": "Algeria", "value": "DZ"}, 
            {"title": "American Samoa", "value": "AS"}, 
            {"title": "AndorrA", "value": "AD"}, 
            {"title": "Angola", "value": "AO"}, 
            {"title": "Anguilla", "value": "AI"}, 
            {"title": "Antarctica", "value": "AQ"}, 
            {"title": "Antigua and Barbuda", "value": "AG"}, 
            {"title": "Argentina", "value": "AR"}, 
            {"title": "Armenia", "value": "AM"}, 
            {"title": "Aruba", "value": "AW"}, 
            {"title": "Australia", "value": "AU"}, 
            {"title": "Austria", "value": "AT"}, 
            {"title": "Azerbaijan", "value": "AZ"}, 
            {"title": "Bahamas", "value": "BS"}, 
            {"title": "Bahrain", "value": "BH"}, 
            {"title": "Bangladesh", "value": "BD"}, 
            {"title": "Barbados", "value": "BB"}, 
            {"title": "Belarus", "value": "BY"}, 
            {"title": "Belgium", "value": "BE"}, 
            {"title": "Belize", "value": "BZ"}, 
            {"title": "Benin", "value": "BJ"}, 
            {"title": "Bermuda", "value": "BM"}, 
            {"title": "Bhutan", "value": "BT"}, 
            {"title": "Bolivia", "value": "BO"}, 
            {"title": "Bosnia and Herzegovina", "value": "BA"}, 
            {"title": "Botswana", "value": "BW"}, 
            {"title": "Bouvet Island", "value": "BV"}, 
            {"title": "Brazil", "value": "BR"}, 
            {"title": "British Indian Ocean Territory", "value": "IO"}, 
            {"title": "Brunei Darussalam", "value": "BN"}, 
            {"title": "Bulgaria", "value": "BG"}, 
            {"title": "Burkina Faso", "value": "BF"}, 
            {"title": "Burundi", "value": "BI"}, 
            {"title": "Cambodia", "value": "KH"}, 
            {"title": "Cameroon", "value": "CM"}, 
            {"title": "Canada", "value": "CA"}, 
            {"title": "Cape Verde", "value": "CV"}, 
            {"title": "Cayman Islands", "value": "KY"}, 
            {"title": "Central African Republic", "value": "CF"}, 
            {"title": "Chad", "value": "TD"}, 
            {"title": "Chile", "value": "CL"}, 
            {"title": "China", "value": "CN"}, 
            {"title": "Christmas Island", "value": "CX"}, 
            {"title": "Cocos (Keeling) Islands", "value": "CC"}, 
            {"title": "Colombia", "value": "CO"}, 
            {"title": "Comoros", "value": "KM"}, 
            {"title": "Congo", "value": "CG"}, 
            {"title": "Congo, The Democratic Republic of the", "value": "CD"}, 
            {"title": "Cook Islands", "value": "CK"}, 
            {"title": "Costa Rica", "value": "CR"}, 
            {"title": "Cote D\"Ivoire", "value": "CI"}, 
            {"title": "Croatia", "value": "HR"}, 
            {"title": "Cuba", "value": "CU"}, 
            {"title": "Cyprus", "value": "CY"}, 
            {"title": "Czech Republic", "value": "CZ"}, 
            {"title": "Denmark", "value": "DK"}, 
            {"title": "Djibouti", "value": "DJ"}, 
            {"title": "Dominica", "value": "DM"}, 
            {"title": "Dominican Republic", "value": "DO"}, 
            {"title": "Ecuador", "value": "EC"}, 
            {"title": "Egypt", "value": "EG"}, 
            {"title": "El Salvador", "value": "SV"}, 
            {"title": "Equatorial Guinea", "value": "GQ"}, 
            {"title": "Eritrea", "value": "ER"}, 
            {"title": "Estonia", "value": "EE"}, 
            {"title": "Ethiopia", "value": "ET"}, 
            {"title": "Falkland Islands (Malvinas)", "value": "FK"}, 
            {"title": "Faroe Islands", "value": "FO"}, 
            {"title": "Fiji", "value": "FJ"}, 
            {"title": "Finland", "value": "FI"}, 
            {"title": "France", "value": "FR"}, 
            {"title": "French Guiana", "value": "GF"}, 
            {"title": "French Polynesia", "value": "PF"}, 
            {"title": "French Southern Territories", "value": "TF"}, 
            {"title": "Gabon", "value": "GA"}, 
            {"title": "Gambia", "value": "GM"}, 
            {"title": "Georgia", "value": "GE"}, 
            {"title": "Germany", "value": "DE"}, 
            {"title": "Ghana", "value": "GH"}, 
            {"title": "Gibraltar", "value": "GI"}, 
            {"title": "Greece", "value": "GR"}, 
            {"title": "Greenland", "value": "GL"}, 
            {"title": "Grenada", "value": "GD"}, 
            {"title": "Guadeloupe", "value": "GP"}, 
            {"title": "Guam", "value": "GU"}, 
            {"title": "Guatemala", "value": "GT"}, 
            {"title": "Guernsey", "value": "GG"}, 
            {"title": "Guinea", "value": "GN"}, 
            {"title": "Guinea-Bissau", "value": "GW"}, 
            {"title": "Guyana", "value": "GY"}, 
            {"title": "Haiti", "value": "HT"}, 
            {"title": "Heard Island and Mcdonald Islands", "value": "HM"}, 
            {"title": "Holy See (Vatican City State)", "value": "VA"}, 
            {"title": "Honduras", "value": "HN"}, 
            {"title": "Hong Kong", "value": "HK"}, 
            {"title": "Hungary", "value": "HU"}, 
            {"title": "Iceland", "value": "IS"}, 
            {"title": "India", "value": "IN"}, 
            {"title": "Indonesia", "value": "ID"}, 
            {"title": "Iran, Islamic Republic Of", "value": "IR"}, 
            {"title": "Iraq", "value": "IQ"}, 
            {"title": "Ireland", "value": "IE"}, 
            {"title": "Isle of Man", "value": "IM"}, 
            {"title": "Israel", "value": "IL"}, 
            {"title": "Italy", "value": "IT"}, 
            {"title": "Jamaica", "value": "JM"}, 
            {"title": "Japan", "value": "JP"}, 
            {"title": "Jersey", "value": "JE"}, 
            {"title": "Jordan", "value": "JO"}, 
            {"title": "Kazakhstan", "value": "KZ"}, 
            {"title": "Kenya", "value": "KE"}, 
            {"title": "Kiribati", "value": "KI"}, 
            {"title": "Korea, Democratic People\"S Republic of", "value": "KP"}, 
            {"title": "Korea, Republic of", "value": "KR"}, 
            {"title": "Kuwait", "value": "KW"}, 
            {"title": "Kyrgyzstan", "value": "KG"}, 
            {"title": "Lao People\"S Democratic Republic", "value": "LA"}, 
            {"title": "Latvia", "value": "LV"}, 
            {"title": "Lebanon", "value": "LB"}, 
            {"title": "Lesotho", "value": "LS"}, 
            {"title": "Liberia", "value": "LR"}, 
            {"title": "Libyan Arab Jamahiriya", "value": "LY"}, 
            {"title": "Liechtenstein", "value": "LI"}, 
            {"title": "Lithuania", "value": "LT"}, 
            {"title": "Luxembourg", "value": "LU"}, 
            {"title": "Macao", "value": "MO"}, 
            {"title": "Macedonia, The Former Yugoslav Republic of", "value": "MK"}, 
            {"title": "Madagascar", "value": "MG"}, 
            {"title": "Malawi", "value": "MW"}, 
            {"title": "Malaysia", "value": "MY"}, 
            {"title": "Maldives", "value": "MV"}, 
            {"title": "Mali", "value": "ML"}, 
            {"title": "Malta", "value": "MT"}, 
            {"title": "Marshall Islands", "value": "MH"}, 
            {"title": "Martinique", "value": "MQ"}, 
            {"title": "Mauritania", "value": "MR"}, 
            {"title": "Mauritius", "value": "MU"}, 
            {"title": "Mayotte", "value": "YT"}, 
            {"title": "Mexico", "value": "MX"}, 
            {"title": "Micronesia, Federated States of", "value": "FM"}, 
            {"title": "Moldova, Republic of", "value": "MD"}, 
            {"title": "Monaco", "value": "MC"}, 
            {"title": "Mongolia", "value": "MN"}, 
            {"title": "Montenegro", "value": "ME"},
            {"title": "Montserrat", "value": "MS"},
            {"title": "Morocco", "value": "MA"}, 
            {"title": "Mozambique", "value": "MZ"}, 
            {"title": "Myanmar", "value": "MM"}, 
            {"title": "Namibia", "value": "NA"}, 
            {"title": "Nauru", "value": "NR"}, 
            {"title": "Nepal", "value": "NP"}, 
            {"title": "Netherlands", "value": "NL"}, 
            {"title": "Netherlands Antilles", "value": "AN"}, 
            {"title": "New Caledonia", "value": "NC"}, 
            {"title": "New Zealand", "value": "NZ"}, 
            {"title": "Nicaragua", "value": "NI"}, 
            {"title": "Niger", "value": "NE"}, 
            {"title": "Nigeria", "value": "NG"}, 
            {"title": "Niue", "value": "NU"}, 
            {"title": "Norfolk Island", "value": "NF"}, 
            {"title": "Northern Mariana Islands", "value": "MP"}, 
            {"title": "Norway", "value": "NO"}, 
            {"title": "Oman", "value": "OM"}, 
            {"title": "Pakistan", "value": "PK"}, 
            {"title": "Palau", "value": "PW"}, 
            {"title": "Palestinian Territory, Occupied", "value": "PS"}, 
            {"title": "Panama", "value": "PA"}, 
            {"title": "Papua New Guinea", "value": "PG"}, 
            {"title": "Paraguay", "value": "PY"}, 
            {"title": "Peru", "value": "PE"}, 
            {"title": "Philippines", "value": "PH"}, 
            {"title": "Pitcairn", "value": "PN"}, 
            {"title": "Poland", "value": "PL"}, 
            {"title": "Portugal", "value": "PT"}, 
            {"title": "Puerto Rico", "value": "PR"}, 
            {"title": "Qatar", "value": "QA"}, 
            {"title": "Reunion", "value": "RE"}, 
            {"title": "Romania", "value": "RO"}, 
            {"title": "Russian Federation", "value": "RU"}, 
            {"title": "RWANDA", "value": "RW"}, 
            {"title": "Saint Helena", "value": "SH"}, 
            {"title": "Saint Kitts and Nevis", "value": "KN"}, 
            {"title": "Saint Lucia", "value": "LC"}, 
            {"title": "Saint Pierre and Miquelon", "value": "PM"}, 
            {"title": "Saint Vincent and the Grenadines", "value": "VC"}, 
            {"title": "Samoa", "value": "WS"}, 
            {"title": "San Marino", "value": "SM"}, 
            {"title": "Sao Tome and Principe", "value": "ST"}, 
            {"title": "Saudi Arabia", "value": "SA"}, 
            {"title": "Senegal", "value": "SN"}, 
            {"title": "Serbia", "value": "RS"}, 
            {"title": "Seychelles", "value": "SC"}, 
            {"title": "Sierra Leone", "value": "SL"}, 
            {"title": "Singapore", "value": "SG"}, 
            {"title": "Slovakia", "value": "SK"}, 
            {"title": "Slovenia", "value": "SI"}, 
            {"title": "Solomon Islands", "value": "SB"}, 
            {"title": "Somalia", "value": "SO"}, 
            {"title": "South Africa", "value": "ZA"}, 
            {"title": "South Georgia and the South Sandwich Islands", "value": "GS"}, 
            {"title": "Spain", "value": "ES"}, 
            {"title": "Sri Lanka", "value": "LK"}, 
            {"title": "Sudan", "value": "SD"}, 
            {"title": "Surititle", "value": "SR"}, 
            {"title": "Svalbard and Jan Mayen", "value": "SJ"}, 
            {"title": "Swaziland", "value": "SZ"}, 
            {"title": "Sweden", "value": "SE"}, 
            {"title": "Switzerland", "value": "CH"}, 
            {"title": "Syrian Arab Republic", "value": "SY"}, 
            {"title": "Taiwan, Province of China", "value": "TW"}, 
            {"title": "Tajikistan", "value": "TJ"}, 
            {"title": "Tanzania, United Republic of", "value": "TZ"}, 
            {"title": "Thailand", "value": "TH"}, 
            {"title": "Timor-Leste", "value": "TL"}, 
            {"title": "Togo", "value": "TG"}, 
            {"title": "Tokelau", "value": "TK"}, 
            {"title": "Tonga", "value": "TO"}, 
            {"title": "Trinidad and Tobago", "value": "TT"}, 
            {"title": "Tunisia", "value": "TN"}, 
            {"title": "Turtitle", "value": "TR"}, 
            {"title": "Turkmenistan", "value": "TM"}, 
            {"title": "Turks and Caicos Islands", "value": "TC"}, 
            {"title": "Tuvalu", "value": "TV"}, 
            {"title": "Uganda", "value": "UG"}, 
            {"title": "Ukraine", "value": "UA"}, 
            {"title": "United Arab Emirates", "value": "AE"}, 
            {"title": "United Kingdom", "value": "GB"}, 
            {"title": "United States", "value": "US"}, 
            {"title": "United States Minor Outlying Islands", "value": "UM"}, 
            {"title": "Uruguay", "value": "UY"}, 
            {"title": "Uzbekistan", "value": "UZ"}, 
            {"title": "Vanuatu", "value": "VU"}, 
            {"title": "Venezuela", "value": "VE"}, 
            {"title": "Viet Nam", "value": "VN"}, 
            {"title": "Virgin Islands, British", "value": "VG"}, 
            {"title": "Virgin Islands, U.S.", "value": "VI"}, 
            {"title": "Wallis and Futuna", "value": "WF"}, 
            {"title": "Western Sahara", "value": "EH"}, 
            {"title": "Yemen", "value": "YE"}, 
            {"title": "Zambia", "value": "ZM"}, 
            {"title": "Zimbabwe", "value": "ZW"} 
      ];
      const searchItems = ref(dropdownItems); // search Result
      const showSingleSelect = ref(true);
      const resetSingleDropdown = {"title": "India", "value": "IN"};
      const resetMultiDropdown = [
        {"title": "Afghanistan", "value": "AF"},
        {"title": "India", "value": "IN"}
      ]
      // Methods

      const handleSelectedItems = (data) => {
        //  Function to handle the response from the dropdown

        console.log('handleSelectedItems', data)
      }

      const handleSearchItems = (searchKey) => {
        // Function to handle search value, you call any api from here or just filter the data
        if (searchKey) {
          searchItems.value = dropdownItems.filter(item => item.title.toLowerCase().includes(searchKey.toLowerCase()))
        } else searchItems.value = dropdownItems;
      }

      // Computed

      const getdropdownItems = computed(() => {
        return searchItems.value;
      })

      return  {
        // Data
        dropdownItems,
        searchItems,
        showSingleSelect, 
        resetSingleDropdown,
        resetMultiDropdown,

        // Methods
        handleSelectedItems,
        handleSearchItems,

        // computed
        getdropdownItems
      }
    }
  };
  </script>
