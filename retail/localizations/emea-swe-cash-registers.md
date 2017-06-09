# Cash registers for Sweden
### Setup guide for the POS localization for Sweden

Using the Microsoft Dynamics POS solution requires some preliminarily settings being maintained in the Retail module of Dynamics365 for Operations.
For the demonstration, training and testing purposes it's possible to use a standard demo data provided by Microsoft. For setting up Retail module from the scratch as well as for making additional settings based on existing dataset you may use this wiki-page: https://docs.microsoft.com/en-us/dynamics365/operations/retail/. 

It explains the most frequently used and the most important preparation steps. Additional Retail module settings specific for Sweden are described below.
- Set up the Swedish country-context in the current company by updating a primary address in the related Legal entity with 'SWE' country/region code. 
- Maintain VAT settings in accordance with effective legislation requirements. Regarding Dynamics365 for Operations it means to set up the following:
  -	create sales tax codes with an appropriate tax rates,
  -	include sales tax codes in new or existing sales tax groups and item sales tax groups.

Item sales tax groups should be specified  for released products.
More information about the sales tax settings: 
https://docs.microsoft.com/en-us/dynamics365/operations/financials/general-ledger/indirect-taxes-overview.
 
- Update a retail store details on the form 'All retail stores'. Update sales tax parameters, fill 'Tax identification number (TIN)' with a company identity number (The Swedish organization number of the dealer).
Field 'Store name' should include the company name.
- Update POS permissions on the form 'Permission groups' if needed. Additionally, here you might limit the number of receipt copies allowed for printing and grant a store manager the permission to skip the fiscal registration of transactions if such registration is currently unavailable.
- Update a POS functionality profile on the form 'Functionality profiles':
  - set Swedish ISO code ('SE').
  - prohibit mixing sales and returns in one receipt,
  - enable a receipt copy registration,
  - specify parameters of the receipt numbering.
- Make necessary changes in the receipt format on the form ''Receipt profiles:
  - set up Language texts and Custom fields,
  - define the reprint message and custom fields in the receipt layout,
  - add fields to display tax amounts per tax rate.
- Setup a fiscal register configuration in xml format. It should contain a mapping of control unit TaxId fields with an appropriate sales tax code from Dynamics365 for Operations. You can use the example below to create your own configuration:

    <?xml version="1.0" encoding="utf-8"?>
    	<configuration>
    	  <configSections>
    	    <section name="UnitConfiguration" type="Microsoft.Dynamics.Retail.FiscalRegistrationServices.CleanCashFiscalRegister.UnitConfiguration, Microsoft.Dynamics.Retail.FiscalRegistrationServices.CleanCashFiscalRegister" />
    	  </configSections>
    	  <UnitConfiguration>
    	    <!--Setting up mapping between VAT codes in POS and control unit that contain VAT rates.-->
    	    <TaxMapping>
    	      <Tax taxCode="stc1" controlUnitTaxId="1" />
    	      <Tax taxCode="stc2" controlUnitTaxId="2" />
    	    </TaxMapping>
    	  </UnitConfiguration>
    </configuration> 
       
Note: stc1, stc2 - sales tax codes with a different tax rate; substitute them with sales tax codes from your dataset.

- Maintain a hardware profile:
Maintain a hardware profile:
  - set up parameters for integration with a control unit, select the fiscal register configuration created before; 
  - specify default options for receipt printing.


5. Create and assign a fiscal register configuration in Retail Headquarters:

    - Run Dynamics 365 for Retail Headquarters.

    - Open *Retail > Channel setup > POS setup > POS profiles > Fiscal register configurations*

    - Create a new fiscal register configuration record, set the name and the description of the configuration.

    - Fill in the configuration content. For this sample, a configuration is an XML file that establishes the mapping between sales tax codes and control unit's VAT groups. You can map up to four sales tax codes. An example configuration can be found below. It is also possible to export a sample configuration by selecting *Export sample configuration* in the action pane.

        ``` xml
        <UnitConfiguration>
            <TaxMapping>
                <Tax taxCode="VAT10" controlUnitTaxId="1"/>
                <Tax taxCode="VAT20" controlUnitTaxId="2"/>
            </TaxMapping>
        </UnitConfiguration>
        ```
    
    - Open *Retail > Channel setup > POS setup > POS profiles > Hardware profiles*

    - Select the hardware profile of the HW station, which the fiscal register is connected to, and set the following fields on the Fisacl register fast-tab:

        - Select Third-party driver in the Fiscal register field;

        - Select the name of the newly created fiscal register configuration in the Configuration field.
