# all-in-one-jks
all items delivery at door step
import React, { useState } from "react";

const data = {
  eCommerce: ["groceries", "milk", "meat", "fruits", "vegetables", "flowers", "others"],
  food: ["biriyani", "iceCream", "pizzaBurger", "drinks", "others"],
  pharmacy: ["medicines", "healthProducts", "supplements", "others"],
  others: ["service1", "service2", "service3", "others"],
};

const profileFields = ["name", "age", "location", "state", "pincode"];

function App() {
  const [category, setCategory] = useState("eCommerce");
  const [subcategory, setSubcategory] = useState(data.eCommerce[0]);
  const [orderIndex, setOrderIndex] = useState(0);
  const [orders, setOrders] = useState(() => {
    const initial = {};
    Object.entries(data).forEach(([cat, subs]) => {
      initial[cat] = {};
      subs.forEach((sub) => {
        initial[cat][sub] = Array(10).fill({ name: "", mobile: "", orderDetails: "", deliveryAddress: "" }).map((o) => ({ ...o }));
      });
    });
    return initial;
  });

  const [profile, setProfile] = useState({
    name: "",
    age: "",
    location: "",
    state: "",
    pincode: "",
  });

  const [addCashOption, setAddCashOption] = useState(null);

  // Handlers
  const handleCategoryChange = (cat) => {
    setCategory(cat);
    setSubcategory(data[cat][0]);
    setOrderIndex(0);
  };

  const handleSubcategoryChange = (subcat) => {
    setSubcategory(subcat);
    setOrderIndex(0);
  };

  const handleOrderChange = (e) => {
    const { name, value } = e.target;
    setOrders((prev) => {
      const copy = { ...prev };
      copy[category][subcategory][orderIndex][name] = value;
      return copy;
    });
  };

  const handleProfileChange = (e) => {
    const { name, value } = e.target;
    setProfile((prev) => ({ ...prev, [name]: value }));
  };

  const placeOrder = () => {
    const order = orders[category][subcategory][orderIndex];
    if (!order.name || !order.mobile || !order.deliveryAddress) {
      alert("Fill Name, Mobile & Delivery Address.");
      return;
    }
    const message = `Order Details:
Category: ${category}
Subcategory: ${subcategory}
Name: ${order.name}
Mobile: ${order.mobile}
Order Details: ${order.orderDetails}
Delivery Address: ${order.deliveryAddress}`;
    const whatsappURL = `https://wa.me/918977143043?text=${encodeURIComponent(message)}`;
    window.open(whatsappURL, "_blank");
  };

  const openGoogleMaps = () => {
    window.open("https://www.google.com/maps", "_blank");
  };

  const openCamera = () => {
    navigator.mediaDevices
      .getUserMedia({ video: true })
      .then((stream) => {
        const videoWindow = window.open("", "", "width=640,height=480");
        const videoElem = videoWindow.document.createElement("video");
        videoElem.autoplay = true;
        videoElem.srcObject = stream;
        videoWindow.document.body.appendChild(videoElem);
      })
      .catch(() => alert("Camera access denied or not available."));
  };

  const addCash = (method) => {
    alert(`Add cash selected: ${method} (Integrate payment gateway here)`);
    setAddCashOption(null);
  };

  return (
    <div style={{ fontFamily: "Arial, sans-serif", backgroundColor: "#e6f2ff", padding: 20 }}>
      <h1>JKS Group Multi-service Platform</h1>

      {/* Navigation */}
      <div style={{ marginBottom: 15 }}>
        <button onClick={() => alert("Navigate Home")} style={{ marginRight: 10 }}>
          Home
        </button>
        <button onClick={openCamera} style={{ marginRight: 10 }}>
          Camera
        </button>
        <button onClick={openGoogleMaps} style={{ marginRight: 10 }}>
          Google Maps
        </button>
        <button onClick={() => alert("Location Setting")} style={{ marginRight: 10 }}>
          Location
        </button>
        <button onClick={() => alert("Orders")} style={{ marginRight: 10 }}>
          Orders
        </button>
        <button onClick={() => alert("Profile Section")} style={{ marginRight: 10 }}>
          Profile
        </button>
        <button onClick={() => setAddCashOption(true)}>Add Cash</button>
      </div>

      {/* Category selection */}
      <div style={{ marginBottom: 15 }}>
        {Object.keys(data).map((cat) => (
          <button
            key={cat}
            onClick={() => handleCategoryChange(cat)}
            style={{ marginRight: 10, backgroundColor: category === cat ? "#66b3ff" : "white" }}
          >
            {cat.toUpperCase()}
          </button>
        ))}
      </div>

      {/* Subcategory selection */}
      <div style={{ marginBottom: 15 }}>
        {data[category].map((sub) => (
          <button
            key={sub}
            onClick={() => handleSubcategoryChange(sub)}
            style={{ marginRight: 10, backgroundColor: subcategory === sub ? "#66b3ff" : "white" }}
          >
            {sub.toUpperCase()}
          </button>
        ))}
      </div>

      {/* Order form */}
      <div style={{ padding: 10, backgroundColor: "white", maxWidth: 500 }}>
        <h3>
          {subcategory.toUpperCase()} Order {orderIndex + 1} / 10
        </h3>
        <input
          type="text"
          placeholder="Name"
          name="name"
          value={orders[category][subcategory][orderIndex].name}
          onChange={handleOrderChange}
          style={{ width: "100%", marginBottom: 10 }}
        />
        <input
          type="text"
          placeholder="Mobile Number"
          name="mobile"
          value={orders[category][subcategory][orderIndex].mobile}
          onChange={handleOrderChange}
          style={{ width: "100%", marginBottom: 10 }}
        />
        <input
          type="text"
          placeholder="Order Details"
          name="orderDetails"
          value={orders[category][subcategory][orderIndex].orderDetails}
          onChange={handleOrderChange}
          style={{ width: "100%", marginBottom: 10 }}
        />
        <input
          type="text"
          placeholder="Delivery Address"
          name="deliveryAddress"
          value={orders[category][subcategory][orderIndex].deliveryAddress}
          onChange={handleOrderChange}
          style={{ width: "100%", marginBottom: 10 }}
        />

        <button onClick={placeOrder} style={{ width: "100%", backgroundColor: "#3399ff", color: "white" }}>
          Place Order via WhatsApp
        </button>

        <div style={{ marginTop: 10 }}>
          <button onClick={() => setOrderIndex(Math.max(orderIndex - 1, 0))} disabled={orderIndex === 0} style={{ marginRight: 10 }}>
            Previous
          </button>
          <button onClick={() => setOrderIndex(Math.min(orderIndex + 1, 9))} disabled={orderIndex === 9}>
            Next
          </button>
        </div>
      </div>

      {/* Profile Section */}
      <div style={{ marginTop: 40, padding: 15, backgroundColor: "white", maxWidth: 500 }}>
        <h3>Profile</h3>
        {profileFields.map((field) => (
          <input
            key={field}
            type={field === "age" ? "number" : "text"}
            placeholder={field.charAt(0).toUpperCase() + field.slice(1)}
            name={field}
            value={profile[field]}
            onChange={handleProfileChange}
            style={{ width: "100%", marginBottom: 10 }}
          />
        ))}
      </div>

      {/* Add Cash Section */}
      {addCashOption && (
        <div style={{ marginTop: 40, padding: 15, backgroundColor: "white", maxWidth: 300 }}>
          <h3>Add Cash</h3>
          <button onClick={() => addCash("PhonePe")} style={{ width: "100%", marginBottom: 10 }}>
            PhonePe
          </button>
          <button onClick={() => addCash("Google Pay")} style={{ width: "100%" }}>
            Google Pay
          </button>
          <button onClick={() => setAddCashOption(false)} style={{ marginTop: 10 }}>
            Cancel
          </button>
        </div>
      )}
    </div>
  );
}

export default App;
