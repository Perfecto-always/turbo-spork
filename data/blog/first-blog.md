---
title: Why is my exit animation in Framer Motion not firing or showing?
date: January 2, 2022 8:34 PM
tags:
  - framer-motion
  - animations
  - react
  - modal
draft: false
summary: How to fix exit animation in framer motion using react.
layout: PostLayout
---
I was trying to create an Animated Modal in React using Framer Motion. Everything was going smoothly until I noticed the reason of using Framer Motion, that is, animating when the component is unmounted from the DOM.

The starting animation was working fine but I noticed that the unmounting animations were really odd until I realized that it is not even working.

The **reason** for exit animation to not fire could be: 

* You might have forgot to even write an \`exit\` animation in the first place. But as you are reading this I doubt you not double checking it. You could also use \`variants\` for making it easy to manage complex animations.
* Another reason could be not wrapping you component with `<AnimatePresence>`. It is necessary to wrap your component with it as it says on their [site](https://www.framer.com/docs/animate-presence/).

  > `AnimatePresence` allows components to animate out when they're removed from the React tree.
 
* And finally, what happened to me was I was not having a \`motion\` tag as an immediate child of \`AnimatePresence\`

Below is the working code of animating Modal in React js. 

```javascript
import { AnimatePresence, motion } from "framer-motion";
import { useEffect } from "react";

function Modal({ open, close }) {
  useEffect(() => {
    const timeout = setTimeout(() => {
      close(false);
    }, 2000);
    return () => clearTimeout(timeout);
  }, [open, close]);

  return (
    <AnimatePresence exitBeforeEnter>
      {open && (
        <motion.div
          onClick={() => {
            close(false);
          }}
          key="modal"
          initial={{ opacity: 0 }}
          animate={{
            opacity: 1
          }}
          exit={{ opacity: 0 }}
          transition={{ duration: 0.2, ease: "linear" }}
          style={{
            width: "100%",
            height: "100vh",
            backgroundColor: "rgba(0, 0, 0,0.2)",
            position: "absolute",
            top: 0,
            left: 0,
            right: 0,
            bottom: 0,
            display: "flex",
            justifyContent: "center",
            alignItems: "center",
            overflow: "hidden"
          }}
        >
          <motion.div
            layout
            initial={{ opacity: 0, scaleY: 0, scaleX: 0 }}
            animate={{ opacity: 1, scaleY: 1, scaleX: 1 }}
            exit={{ opacity: 0, scaleY: 0, scaleX: 0 }}
            transition={{ duration: 0.2 }}
            style={{
              backgroundColor: "white",
              padding: "2rem",
              zIndex: "99999999",
              overflow: "hidden",
              textAlign: "center",
              borderRadius: "5px"
            }}
          >
            This is a white modal
          </motion.div>
        </motion.div>
      )}
    </AnimatePresence>
  );
}

export default Modal;
```

Or here is the link of [codesandbox](https://codesandbox.io/s/animated-modal-0cr72) if you want to experiment with Framer Motion further yourself.
