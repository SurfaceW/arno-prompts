# You are a CSS Assistant.

* I use `tailwindcss` css framework

## Problem Define

I want to implement a popover and its background should use `backdrop:blur` to have cool effects.

## My implemented code is here:

```html
<div className="w-full h-full overflow-hidden z-0 dark:bg-black bg-white backdrop:blur"></div>
  <div
    id="slash-command"
    ref={commandListContainer}
    className="z-50 h-auto max-h-[330px] w-72 overflow-y-auto rounded-md bg-white dark:bg-transparent px-1 py-2 shadow-md dark:shadow-none transition-all dark:border dark:border-slate-600 dark:rounded-md">
    {items.map((item: CommandItemProps, index: number) => {
      return (
        <button
          className={`flex w-full items-center space-x-2 rounded-md px-2 py-1 text-left text-sm text-gray-900 dark:text-white hover:bg-stone-100 dark:hover:bg-slate-800 ${index === selectedIndex
            ? "bg-stone-100 dark:bg-slate-900 text-stone-900 dark:text-white"
            : ""
            }`}
          key={index}
          onClick={() => selectItem(index)}>
          <div className="flex h-10 w-10 items-center justify-center rounded-md border border-stone-200 bg-white dark:bg-slate-900">
            {item.title === "Continue writing" && false ? (
              <Skeleton active />
            ) : (
              item.icon
            )}
          </div>
          <div>
            <p className="font-medium">{item.title}</p>
            <p className="text-xs text-white-500">
              {item.description}
            </p>
          </div>
          {item.title === "Continue writing" && false && (
            <div>
              <PauseCircleOutlined
                className="h-5 w-5 text-white-300 hover:text-stone-500 cursor-pointer"
                onClick={stop}
              />
            </div>
          )}
        </button>
      );
    })}
  </div>
</div>
```

## Goal

can you help me modifiy the existed code and tell me how to implement such feature?
