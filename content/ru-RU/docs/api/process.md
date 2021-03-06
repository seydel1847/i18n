# process

> Extensions to process object.

Процессы: [Main](../glossary.md#main-process), [Renderer](../glossary.md#renderer-process)

Объект Electron `process` является расширенной версией объекта [Node.js `process`](https://nodejs.org/api/process.html). Он добавляет следующие события, свойства и методы:

## Sandbox

In sandboxed renderers the `process` object contains only a subset of the APIs:

* `crash()`
* `hang()`
* `getHeapStatistics()`
* `getSystemMemoryInfo()`
* `getCPUUsage()`
* `getIOCounters()`
* `argv`
* `execPath`
* `env`
* `pid`
* `arch`
* `platform`
* `resourcesPath`
* `sandboxed`
* `тип`
* `version`
* `versions`
* `mas`
* `windowsStore`

## События

### Событие: 'loaded'

Срабатывает, когда Electron загрузил свой скрипт внутренней инициализации и начинает загружать веб-страницу или основной скрипт.

Это событие может использоваться preload скриптом, чтобы вернуть удаленные Node global symbols в глобальную область видимости, когда node integration выключен:

```javascript
// preload.js
const _setImmediate = setImmediate
const _clearImmediate = clearImmediate
process.once('loaded', () => {
  global.setImmediate = _setImmediate
  global.clearImmediate = _clearImmediate
})
```

## Свойства

### `process.defaultApp`

`Boolean`. Когда app запущено, будучи переданным в качестве параметра в default app, это свойство принимает значение `true` в main process, иначе `undefined`.

### `process.mas`

`Boolean`. Для Mac App Store сборки это свойство `true`, для остальных сборок `undefined`.

### `process.noAsar`

`Boolean`. Контролирует поддержку ASAR внутри вашего приложения. Установка данного параметра в `true` отключит поддержку `asar` архивов в Node's built-in модулях.

### `process.noDeprecation`

`Boolean`. Контролирует будут ли неодобряющие предупреждения выводиться в `stderr`. Установка в `true` заглушит неодобряющие предупреждения. Это свойство используется вместо флага командной строки `--no-deprecation`.

### `process.resourcesPath`

`String`. Представляет из себя путь до каталога с ресурсами.

### `process.sandboxed`

`Boolean`. Когда renderer process добавлен в sandbox это свойство принимает значение `true`, иначе `undefined`.

### `process.throwDeprecation`

A `Boolean` that controls whether or not deprecation warnings will be thrown as exceptions. Setting this to `true` will throw errors for deprecations. This property is used instead of the `--throw-deprecation` command line flag.

### `process.traceDeprecation`

A `Boolean` that controls whether or not deprecations printed to `stderr` include their stack trace. Setting this to `true` will print stack traces for deprecations. This property is instead of the `--trace-deprecation` command line flag.

### `process.traceProcessWarnings`

A `Boolean` that controls whether or not process warnings printed to `stderr` include their stack trace. Setting this to `true` will print stack traces for process warnings (including deprecations). This property is instead of the `--trace-warnings` command line flag.

### `process.type`

A `String` representing the current process's type, can be `"browser"` (i.e. main process) or `"renderer"`.

### `process.versions.chrome`

A `String` representing Chrome's version string.

### `process.versions.electron`

A `String` representing Electron's version string.

### `process.windowsStore`

A `Boolean`. If the app is running as a Windows Store app (appx), this property is `true`, for otherwise it is `undefined`.

## Методы

The `process` object has the following methods:

### `process.crash()`

Causes the main thread of the current process crash.

### `process.getCreationTime()`

Returns `Number | null` - The number of milliseconds since epoch, or `null` if the information is unavailable

Indicates the creation time of the application. The time is represented as number of milliseconds since epoch. It returns null if it is unable to get the process creation time.

### `process.getCPUUsage()`

Returns [`CPUUsage`](structures/cpu-usage.md)

### `process.getIOCounters()` *Windows* *Linux*

Returns [`IOCounters`](structures/io-counters.md)

### `process.getHeapStatistics()`

Возвращает `Object`:

* `totalHeapSize` Integer
* `totalHeapSizeExecutable` Integer
* `totalPhysicalSize` Integer
* `totalAvailableSize` Integer
* `usedHeapSize` Integer
* `heapSizeLimit` Integer
* `mallocedMemory` Integer
* `peakMallocedMemory` Integer
* `doesZapGarbage` Boolean

Returns an object with V8 heap statistics. Note that all statistics are reported in Kilobytes.

### `process.getSystemMemoryInfo()`

Возвращает `Object`:

* `total` Integer - The total amount of physical memory in Kilobytes available to the system.
* `free` Integer - The total amount of memory not being used by applications or disk cache.
* `swapTotal` Integer *Windows* *Linux* - The total amount of swap memory in Kilobytes available to the system.
* `swapFree` Integer *Windows* *Linux* - The free amount of swap memory in Kilobytes available to the system.

Returns an object giving memory usage statistics about the entire system. Note that all statistics are reported in Kilobytes.

### `process.takeHeapSnapshot(filePath)`

* `filePath` String - Path to the output file.

Returns `Boolean` - Indicates whether the snapshot has been created successfully.

Takes a V8 heap snapshot and saves it to `filePath`.

### `process.hang()`

Causes the main thread of the current process hang.

### `process.setFdLimit(maxDescriptors)` *macOS* *Linux*

* `maxDescriptors` Integer

Sets the file descriptor soft limit to `maxDescriptors` or the OS hard limit, whichever is lower for the current process.