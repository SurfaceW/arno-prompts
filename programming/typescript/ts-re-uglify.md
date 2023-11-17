# Anti Uglify Assistant

- author: Arno
- date: 2023-11-13
- version: 0.1.0
- models: long-text use 32k / 16k model
- pattern: completion

## Description

Anti Uglify Assistant is a tool to help you trace those uglify code and make it more readable though typescript.

## Prompt Structure

I give the following compiled and uglified javascript source code:

```javascript
import {
  Extension as t,
  findChildren as e,
  combineTransactionSteps as r,
  getChangedRanges as n,
  findChildrenInRange as i,
  findDuplicates as o,
} from '@tiptap/core';
import { Plugin as a, PluginKey as s } from '@tiptap/pm/state';
import { Slice as d, Fragment as p } from '@tiptap/pm/model';
let UniqueId = new t({
  name: 'unique-id',
  priority: 99999,
  addOptions: () => ({
    attributeName: 'id',
    types: [],
    createId: () => window.crypto.randomUUID(),
  }),
  addGlobalAttributes() {
    return [
      {
        types: this.options.types,
        attributes: {
          [this.options.attributeName]: {
            default: null,
            parseHTML: (t) => t.getAttribute(this.options.attributeName),
            renderHTML: (t) =>
              t[this.options.attributeName]
                ? { [this.options.attributeName]: t[this.options.attributeName] }
                : {},
          },
        },
      },
    ];
  },
  onCreate() {
    let { tr: t, doc: r } = this.editor.state,
      { types: n, attributeName: i, createId: o } = this.options;
    e(r, (t) => n.includes(t.type.name) && null == t.attrs[i]).forEach(({ node: e, pos: r }) =>
      t.setNodeMarkup(r, void 0, { ...e.attrs, [i]: o() })
    ),
      this.editor.view.dispatch(t);
  },
  addProseMirrorPlugins() {
    let t = null,
      e = !1;
    return [
      new a({
        key: new s('unique-id'),
        appendTransaction: (t, { doc: e }, { doc: a, tr: s }) => {
          if (!t.some(({ docChanged: t }) => t) || e.eq(a)) return;
          let { types: d, attributeName: p, createId: u } = this.options,
            l = r(e, t);
          if (
            (n(l).forEach(({ newRange: t }) => {
              let e = i(a, t, (t) => d.includes(t.type.name)),
                r = e.map(({ node: t }) => t.attrs[p]).filter((t) => null !== t);
              e.forEach(({ node: t, pos: e }) => {
                var n;
                let i = null === (n = s.doc.nodeAt(e)) || void 0 === n ? void 0 : n.attrs[p];
                if (null === i) {
                  s.setNodeMarkup(e, void 0, { ...t.attrs, [p]: u() });
                  return;
                }
                l.mapping.invert().mapResult(e) &&
                  o(r).includes(i) &&
                  s.setNodeMarkup(e, void 0, { ...t.attrs, [p]: u() });
              });
            }),
            s.steps.length)
          )
            return s;
        },
        view(e) {
          let r = (r) => {
            var n;
            t = (null === (n = e.dom.parentElement) || void 0 === n ? void 0 : n.contains(r.target))
              ? e.dom.parentElement
              : null;
          };
          return (
            window.addEventListener('dragstart', r),
            {
              destroy() {
                window.removeEventListener('dragstart', r);
              },
            }
          );
        },
        props: {
          handleDOMEvents: {
            drop: (r, n) => {
              var i;
              return (
                (t !== r.dom.parentElement ||
                  (null === (i = n.dataTransfer) || void 0 === i ? void 0 : i.effectAllowed) ===
                    'copy') &&
                  ((t = null), (e = !0)),
                !1
              );
            },
            paste: () => ((e = !0), !1),
          },
          transformPasted: (t) => {
            if (!e) return t;
            let { types: r, attributeName: n } = this.options,
              i = (t) => {
                let e = [];
                return (
                  t.forEach((t) => {
                    if (t.isText) {
                      e.push(t);
                      return;
                    }
                    if (!r.includes(t.type.name)) {
                      e.push(t.copy(i(t.content)));
                      return;
                    }
                    e.push(t.type.create({ ...t.attrs, [n]: null }, i(t.content), t.marks));
                  }),
                  p.from(e)
                );
              };
            return (e = !1), new d(i(t.content), t.openStart, t.openEnd);
          },
        },
      }),
    ];
  },
});
export { UniqueId as default };
```

Try to make it more readable and understandable and output through typescript. one more thing: this code is use lib named `@tiptap` and it is `prosemirror` based, you can reference the type from them

```typescript
{your code here}
```

## Examples

Output of prompt:

```ts
import { Extension, findChildren, combineTransactionSteps, getChangedRanges, findChildrenInRange, findDuplicates } from '@tiptap/core';
import { Plugin, PluginKey } from '@tiptap/pm/state';
import { Slice, Fragment } from '@tiptap/pm/model';

let UniqueId = new Extension({
  name: 'unique-id',
  priority: 99999,
  addOptions: () => ({
    attributeName: 'id',
    types: [],
    createId: () => window.crypto.randomUUID(),
  }),
  addGlobalAttributes() {
    return [
      {
        types: this.options.types,
        attributes: {
          [this.options.attributeName]: {
            default: null,
            parseHTML: (element) => element.getAttribute(this.options.attributeName),
            renderHTML: (element) =>
              element[this.options.attributeName]
                ? { [this.options.attributeName]: element[this.options.attributeName] }
                : {},
          },
        },
      },
    ];
  },
  onCreate() {
    let { tr, doc } = this.editor.state;
    let { types, attributeName, createId } = this.options;
    findChildren(doc, (node) => types.includes(node.type.name) && node.attrs[attributeName] === null).forEach(({ node, pos }) =>
      tr.setNodeMarkup(pos, undefined, { ...node.attrs, [attributeName]: createId() })
    );
    this.editor.view.dispatch(tr);
  },
  addProseMirrorPlugins() {
    let activeElement = null;
    let shouldReset = false;
    return [
      new Plugin({
        key: new PluginKey('unique-id'),
        appendTransaction: (transactions, { doc: prevDoc }, { doc: currentDoc, tr }) => {
          if (!transactions.some(({ docChanged }) => docChanged) || prevDoc.eq(currentDoc)) return;
          let { types, attributeName, createId } = this.options;
          let changedRanges = combineTransactionSteps(prevDoc, transactions);
          getChangedRanges(changedRanges).forEach(({ newRange }) => {
            let elementsInRange = findChildrenInRange(currentDoc, newRange, (node) => types.includes(node.type.name));
            let existingIds = elementsInRange.map(({ node }) => node.attrs[attributeName]).filter((id) => id !== null);
            elementsInRange.forEach(({ node, pos }) => {
              let existingId = currentDoc.nodeAt(pos)?.attrs[attributeName];
              if (existingId === null) {
                tr.setNodeMarkup(pos, undefined, { ...node.attrs, [attributeName]: createId() });
                return;
              }
              let invertedPos = changedRanges.mapping.invert().mapResult(pos);
              if (invertedPos && findDuplicates(existingIds).includes(existingId)) {
                tr.setNodeMarkup(pos, undefined, { ...node.attrs, [attributeName]: createId() });
              }
            });
          });
          if (tr.steps.length) return tr;
        },
        view(view) {
          let handleDragStart = (event) => {
            activeElement = view.dom.parentElement?.contains(event.target) ? view.dom.parentElement : null;
          };
          window.addEventListener('dragstart', handleDragStart);
          return {
            destroy() {
              window.removeEventListener('dragstart', handleDragStart);
            },
          };
        },
        props: {
          handleDOMEvents: {
            drop: (view, event) => {
              if (activeElement !== view.dom.parentElement || event.dataTransfer?.effectAllowed === 'copy') {
                activeElement = null;
                shouldReset = true;
              }
              return false;
            },
            paste: () => {
              shouldReset = true;
              return false;
            },
          },
          transformPasted: (slice) => {
            if (!shouldReset) return slice;
            let { types, attributeName } = this.options;
            let transformContent = (nodes) => {
              let transformedNodes = [];
              nodes.forEach((node) => {
                if (node.isText) {
                  transformedNodes.push(node);
                  return;
                }
                if (!types.includes(node.type.name)) {
                  transformedNodes.push(node.copy(transformContent(node.content)));
                  return;
                }
                transformedNodes.push(node.type.create({ ...node.attrs, [attributeName]: null }, transformContent(node.content), node.marks));
              });
              return Fragment.from(transformedNodes);
            };
            shouldReset = false;
            return new Slice(transformContent(slice.content), slice.openStart, slice.openEnd);
          },
        },
      }),
    ];
  },
});

export default UniqueId;
```


## Reference

version: 

- `0.0.1`: [Version]


[Reference]

- [ref content]()