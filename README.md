# **Laptop Request Catalog Item**

### **Problem Statement**

Employees in the organization need a quick and efficient way to request laptops for work. The existing process is manual, time-consuming, and lacks dynamic form behavior, leading to frequent errors and delays. To resolve this, a **Service Catalog Item** is to be created in **ServiceNow**, enabling users to easily request laptops through an intuitive and dynamic form.

The solution will:

* Simplify the laptop request process.
* Automate form behavior with UI Policies.
* Provide a reset option for better user experience.
* Ensure all configuration changes are tracked and deployable between instances.

---

## **Implementation Steps**

### **1. Create Local Update Set**

**Navigation:**
ServiceNow → All → *Update Sets* → *Local Update Sets*

**Steps:**

1. Click **New**.
2. Enter the following details:

   * **Name:** Laptop Request
3. Click **Submit** and then **Make Current**.

🟢 *Note:* All subsequent actions should be performed under this update set.

---

### **2. Create Service Catalog Item**

**Navigation:**
ServiceNow → All → *Service Catalog* → *Maintain Items*

**Steps:**

1. Click **New**.
2. Fill the form with:

   * **Name:** Laptop Request
   * **Catalog:** Service Catalog
   * **Category:** Hardware
   * **Short Description:** Use this item to request a new laptop
3. Click **Save**.

---

### **3. Add Variables**

**Navigation:**
Scroll down to the **Variables (Related List)** section → Click **New**

**Variables to Create:**

| Variable Label         | Type             | Name                   | Order |
| ---------------------- | ---------------- | ---------------------- | ----- |
| Laptop Model           | Single Line Text | laptop_model           | 100   |
| Justification          | Multi Line Text  | justification          | 200   |
| Additional Accessories | Checkbox         | additional_accessories | 300   |
| Accessories Details    | Multi Line Text  | accessories_details    | 400   |

After creating all variables, **save** the catalog item.

---

### **4. Create Catalog UI Policy**

**Navigation:**
ServiceNow → All → *Service Catalog* → *Maintain Items* → Select **Laptop Request** → Scroll to **Catalog UI Policies**

**Steps:**

1. Click **New**.
2. **Short Description:** Show Accessories Details
3. **When to Apply:**

   * Field: additional_accessories
   * Operator: is
   * Value: true
4. Click **Save** (not Submit).
5. Scroll down to **Catalog UI Policy Actions** → Click **New**.

   * **Variable:** accessories_details
   * **Mandatory:** True
   * **Visible:** True
   * **Order:** 100
6. Click **Save**, then **Save** again on the UI Policy form.

---

### **5. Create UI Action (Reset Form)**

**Navigation:**
ServiceNow → All → *UI Actions* (under System Definition)

**Steps:**

1. Click **New**.
2. Enter the following details:

   * **Table:** `sc_cart`
   * **Order:** 100
   * **Action Name:** Reset Form
   * **Client:** Checked
3. Add the following **Script**:

   ```javascript
   function resetForm() {
       g_form.clearForm(); // Clears all fields in the form
       alert("The form has been reset.");
   }
   ```
4. Click **Save**.

---

### **6. Export Update Set**

**Navigation:**
ServiceNow → All → *Update Sets* → *Local Update Sets*

**Steps:**

1. Select the update set **Laptop Request Project**.
2. Set **State = Complete**.
3. In the **Updates** tab, verify all related changes.
4. Click **Export to XML** to download the file.

---

### **7. Retrieve the Update Set (in Another Instance)**

**Navigation:**
ServiceNow → All → *Retrieved Update Sets*

**Steps:**

1. Click **Import Update Set from XML**.
2. Upload the previously downloaded XML file.
3. Click **Upload** → Open the uploaded set.
4. Click **Preview Update Set** → Then **Commit Update Set**.
5. Review the related updates tab to confirm successful import.

---

### **8. Test the Catalog Item**

**Navigation:**
ServiceNow → All → *Service Catalog* → *Hardware Category*

**Steps:**

1. Open the **Laptop Request** item.
2. Verify that the following variables appear:

   * Laptop Model
   * Justification
   * Additional Accessories
3. When the **Additional Accessories** checkbox is selected,
   the **Accessories Details** field should appear and become mandatory.

✅ *If this behavior works as expected, the catalog item is functioning correctly.*

---

### **Project Structure**

```

Laptop-Request-Catalog-Item/
│
├── Update Set/
│  └── Laptop Request (Local Update Set created to capture all configuration changes)
│
├── Service Catalog Item/
│  └── Laptop Request
│    ├── Name: Laptop Request
│    ├── Catalog: Service Catalog
│    ├── Category: Hardware
│    └── Short Description: Use this item to request a new laptop
│
├── Variables/
│  ├── Laptop Model – (Single Line Text)
│  ├── Justification – (Multi Line Text)
│  ├── Additional Accessories – (Checkbox)
│  └── Accessories Details – (Multi Line Text, shown only when checkbox is selected)
│
├── Catalog UI Policy/
│  └── Show Accessories Details
│    ├── Condition: additional_accessories is true
│    └── Action: Make accessories_details visible and mandatory
│
├── UI Action/
│  └── Reset Form
│    ├── Table: sc_cart
│    ├── Client: True
│    └── Script: Clears all form fields and shows an alert message
│
├── Export Update Set/
│  └── Laptop Request Project.xml (exported for migration to other instances)
│
├── Retrieved Update Set/
│  └── Laptop Request Project (imported and committed on target instance)
│
└── Testing/
  └── Verified the Laptop Request catalog item functionality:
    - Checkbox dynamically displays Accessories Details field.
    - Field becomes mandatory when visible.
    - Reset button clears the form successfully.

```
---

### **Outcome**

The **Laptop Request Catalog Item** implementation provides an automated, efficient, and user-friendly way for employees to request laptops through **ServiceNow’s Service Catalog**.

As a result of this project:

* ✅ **Automation & Dynamic Behavior:** The catalog form dynamically adapts to user inputs using UI Policies.
* ✅ **Improved Usability:** Users can easily reset and re-enter details without refreshing the page.
* ✅ **Governance & Traceability:** All configuration changes are tracked through update sets for controlled deployment.
* ✅ **Error Reduction:** Clear fields and validation ensure that requests are complete and accurate before submission.
* ✅ **Faster Fulfillment:** Streamlined request collection enables IT teams to process hardware requests efficiently.

Overall, this project demonstrates the power of ServiceNow’s **Service Catalog** in transforming manual asset request processes into an automated, reliable, and intuitive digital workflow—enhancing both **operational efficiency** and **employee satisfaction**.

---
