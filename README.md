# reactModalwithSidebar
This React app consist of a responsive sidebar and modal.

Constructing this app helped me to better learn and practice using global context and related functions.

A brief walkthrough of the pertinent React code is given below.

To begin the App.js file has all three components added to it.
```React
function App() {
  return (
    <>
      <Home />
      <Modal />
      <Sidebar />
    </>
  );
}
```


Next Home.js is worked on and the two buttons that will control the sidebar and modal installed.
```React
  return (
    <main>
      <button className="sidebar-toggle">
        <FaBars />
      </button>
      <button className="btn">
        show modal
      </button>
    </main>
```


Now either the sidebar or modal can be worked on, this route follows the modal first.
```React
const Modal = () => {
  return (
    <div
      className={`model-overlay show-modal`}
    >
      <div className="modal-container">
        <h3>modal content</h3>
        <button className="close-modal-btn" onClick={closeModal}>
          <FaTimes></FaTimes>
        </button>
      </div>
    </div>
  );
};
```


Still nu functionality currently. The sidebar is setup next and its social and nav links iterated over and rendered dynamic from the data.js file. 
```React
const Sidebar = () => {
  const { isSidebarOpen, closeSidebar } = useGlobalContext();

  return (
    <aside className={`sidebar show-sidebar`}>
      <div className="sidebar-header">
        <h3>🏄‍♀️</h3>
        <button className="close-btn">
          <FaTimes />
        </button>
      </div>
      <ul className="links">
        {links.map((link) => {
          const { id, url, text, icon } = link;
          return (
            <li key={id}>
              <a href={url}>
                {icon}
                {text}
              </a>
            </li>
          );
        })}
      </ul>
      <ul className="social-icons">
        {social.map((link) => {
          const { id, url, icon } = link;
          return (
            <li key={id}>
              <a href={url}>{icon}</a>
            </li>
          );
        })}
      </ul>
    </aside>
  );
};
```


The 'show-sidebar' and 'show-modal' classes being active or not is what determines their being show or not. To toggle these dynamically useState will be used. Since the button to control the modal button is inside oh Home.js, not Modal.js, the useState canot be hooked up inside of Modal. Global context will be used to link these values throughout the application.
To make these values available throughout the application AppContext and AppProvider functions are used from within Context.js.
```React
import React, { useState, useContext } from "react";

const AppContext = React.createContext();

const AppProvider = ({ children }) => {
  const [isSidebarOpen, setIsSidebarOpen] = useState(false);
  const [isModalOpen, setIsModalOpen] = useState(false);

  return (
    <AppContext.Provider
      value={{
        isSidebarOpen,
        isModalOpen
      }}
    >
      {children}
    </AppContext.Provider>


export { AppContext, AppProvider };
```



Then AppProvider is wrapped around the entire application inside of Index.js so that values can be passed throughout the app as required by the global context.
```React
ReactDOM.render(
  <AppProvider>
    <React.StrictMode>
      <App />
    </React.StrictMode>
  </AppProvider>,
  document.getElementById("root")
);


Now components render to screen and in any component the value='' property can be accessed and its inputs imported to other files for use as needed.
useContext is installed and the AppContext function imported and used within it to initialize the data linkage between components.
```React
import { useGlobalContext } from "./context";

const Home = () => {
const data = useContext(AppContext);
console.log(data);
```


Now functionality is set up inside of AppProvider() so that its value can be shared around the app.
```React
import React, { useState, useContext } from "react";

const AppContext = React.createContext();

const AppProvider = ({ children }) => {
  const [isSidebarOpen, setIsSidebarOpen] = useState(false);
  const [isModalOpen, setIsModalOpen] = useState(false);
  ```
  
  
  Now create those four functions.
  ```React
  const openSidebar = () => {
    setIsSidebarOpen(true);
  };
  const closeSidebar = () => {
    setIsSidebarOpen(false);
  };

  const openModal = () => {
    setIsModalOpen(true);
  };
  const closeModal = () => {
    setIsModalOpen(false);
  };
```


Now change the value of AppContext.Provier
```React    <AppContext.Provider
      value={{
        isSidebarOpen,
        isModalOpen,
        openModal,
        closeModal,
        openSidebar,
        closeSidebar,
      }}
    >
```



Set up the buttons in Home.js.
```React
      <button onClick={openSidebar} className="sidebar-toggle">
        <FaBars />
      </button>
      <button onClick={openModal} className="btn">
        show modal
      </button>
```


To make these work values still need to be passed from the context into Modal/Sidebar.js. Modal first.
```React
import { useGlobalContext } from "./context";

const Modal = () => {
  const { isModalOpen, closeModal } = useGlobalContext();
  return (
    <div
      className={`${
        isModalOpen ? "modal-overlay show-modal" : "modal-overlay"
      }`}
    > 
    
 <button className="close-modal-btn" onClick={closeModal}>
   <FaTimes></FaTimes>
 </button>
```


Repeat for the sidebar.
```React;
import { useGlobalContext } from "./context";

const Sidebar = () => {
  const { isSidebarOpen, closeSidebar } = useGlobalContext();

  return (
    <aside className={`${isSidebarOpen ? "sidebar show-sidebar" : "sidebar"}`}>
        <button className="close-btn" onClick={closeSidebar}>
          <FaTimes />
        </button>
    </aside>
  );
};
```


Finally, render to the screen once more - already shown above.


***End walkthrough





