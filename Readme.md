# Speech Recognition and Speech Synthesis C++ with UWP for plain Win32

An easy way to implement Windows SpeechRecognizer and SpeechSynthesizer UWP in your Win32 Apps. Available as DLL and as Static library, in both x64 and x86.

Your library functions are:

```C++
HRESULT __stdcall  SpeechX1(void* ptr, SpeechX2 x2, const wchar_t* langx = L"en-us", int Mode = 0);
HRESULT __stdcall  SpeechX3(const wchar_t* t, void* ptr, bool XML,size_t iVoice);
```

## Text to Speech
Call SpeechX3, passing the text and a vector to return the wave data. As the ptr paramter, pass a pointer to a std::vector<uint8_t>. Use MS Speech Synthesis Markup Language for XML
https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-synthesis-markup?tabs=csharp

The iVoice is the index-0 voice to use. To get all the voices, pass t = 0, then ptr as a pointer std::vector<std::tuple<std::wstring,std::wstring>>>.



## Speech to Text

Mode = 2, pass a std::vector<std::tuple<std::wstring,std::wstring>>> as a ptr to get all languages supported

```C++
std::vector<std::tuple<std::wstring, std::wstring>> sx;
SpeechX1((void*)&sx, 0, 0, 2);
for (auto& e : sx)
{
 std::wcout << std::get<0>(e) << L" - " << std::get<1>(e) << std::endl;
}
```


Continuous dictaction, Mode = 0 and the language specifier with a callback of this type:
```C++
HRESULT __stdcall  MyCallback(void* ptr, const wchar_t* reco, int conf);
SpeechX1(some_ptr, MyCallback, L"en-US", 0);
```
Where
- ptr is the value passed to SpeechX1 as some_ptr
- If reco is nullptr, then the recognizer is in idle state. Return S_OK to continue. If you return E_FAIL, session ends.
- If reco is text, then conf is the confidence mode:
- If -1, it's still in recognition mode
- If 0,1,2,3 high/medium/low/bad confidence

return S_OK to continue, E_FAIL to stop.




