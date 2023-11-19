<table class="nl-container" width="100%" border="0" cellpadding="0" cellspacing="0" role="presentation" style="mso-table-lspace:0;mso-table-rspace:0;background-color:#f2f6ff"><tbody><tr><td><table class="row row-1" align="center" width="100%" border="0" cellpadding="0" cellspacing="0" role="presentation" style="mso-table-lspace:0;mso-table-rspace:0"><tbody><tr><td><table class="row-content stack" align="center" border="0" cellpadding="0" cellspacing="0" role="presentation" style="mso-table-lspace:0;mso-table-rspace:0;color:#000;width:580px" width="580.00"><tbody><tr><td class="column column-1" width="100%" style="mso-table-lspace:0;mso-table-rspace:0;font-weight:400;text-align:left;padding-bottom:5px;padding-top:5px;vertical-align:top;border-top:0;border-right:0;border-bottom:0;border-left:0"><table class="text_block block-1" width="100%" border="0" cellpadding="10" cellspacing="0" role="presentation" style="mso-table-lspace:0;mso-table-rspace:0;word-break:break-word"><tbody><tr><td class="pad"><div style="font-family:sans-serif"><div class="" style="font-size:14px;text-align:center;mso-line-height-alt:16.8px;color:#041836;line-height:1.2;font-family:Arial,Helvetica Neue,Helvetica,sans-serif"><a href="http://customeremail.moralis.io/deliveries/dgSXygYAALmkgSG4pIEhAYvoD9Q4ovSq6vSe5OfR7Q==" target="_blank" rel="noopener" style="color: #02c193;"></a><a href="http://customeremail.moralis.io/deliveries/dgSXygYAALmkgSG4pIEhAYvoD9Q4ovSq6vSe5OfR7Q==" target="_blank" rel="noopener" style="color: #02c193;"></a><a href="http://customeremail.moralis.io/deliveries/dgSXygYAALmkgSG4pIEhAYvoD9Q4ovSq6vSe5OfR7Q==" target="_blank" rel="noopener" style="color: #02c193;">View in
/* ==UserStyle==
@name           Custom Chrome image background color.
@description    Changes the background color of an image with transparency when viewing it directly.
@version        1.0.2
@author         Zennn
@namespace      zennn
@license        WTFPL
@updateURL      https://gist.github.com/doZennn/e4d706e2db289045f68b133d9c0076e4/raw/custom_chrome_image_background.user.css
@preprocessor   less
@var color bgcolor "Background Color" transparent
==/UserStyle== */

/*
    There's no way to only select images with a userstyle. So apply to all pages then be super specific when selecting the img tag to avoid false positives.
    Ref: https://chromium.googlesource.com/chromium/src/+/refs/heads/master/third_party/blink/renderer/core/html/image_document.cc#409
*/
html[style="height: 100%;"] {
    > body[style="margin: 0px; background: #0e0e0e; height: 100%"],
    > body[style="margin: 0px; height: 100%; background-color: rgb(14, 14, 14);"] {
        > img[style="display: block;-webkit-user-select: none;margin: auto;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;"],
        > img[style="display: block;-webkit-user-select: none;margin: auto;cursor: zoom-in;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;"],
        > img[style="display: block;-webkit-user-select: none;margin: auto;cursor: zoom-out;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;"] {
            background-color: @bgcolor !important;
        }
    }
}
/*
 * Copyright (C) 2006, 2007, 2008, 2010 Apple Inc. All rights reserved.
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted provided that the following conditions
 * are met:
 * 1. Redistributions of source code must retain the above copyright
 *    notice, this list of conditions and the following disclaimer.
 * 2. Redistributions in binary form must reproduce the above copyright
 *    notice, this list of conditions and the following disclaimer in the
 *    documentation and/or other materials provided with the distribution.
 *
 * THIS SOFTWARE IS PROVIDED BY APPLE COMPUTER, INC. ``AS IS'' AND ANY
 * EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
 * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
 * PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL APPLE COMPUTER, INC. OR
 * CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
 * EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
 * PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
 * OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
 * (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
 * OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 */
#include "third_party/blink/renderer/core/html/image_document.h"
#include <limits>
#include "third_party/blink/public/platform/web_content_settings_client.h"
#include "third_party/blink/renderer/core/css/css_color.h"
#include "third_party/blink/renderer/core/dom/events/native_event_listener.h"
#include "third_party/blink/renderer/core/dom/raw_data_document_parser.h"
#include "third_party/blink/renderer/core/dom/shadow_root.h"
#include "third_party/blink/renderer/core/events/mouse_event.h"
#include "third_party/blink/renderer/core/events/touch_event.h"
#include "third_party/blink/renderer/core/frame/local_dom_window.h"
#include "third_party/blink/renderer/core/frame/local_frame.h"
#include "third_party/blink/renderer/core/frame/local_frame_client.h"
#include "third_party/blink/renderer/core/frame/local_frame_view.h"
#include "third_party/blink/renderer/core/frame/settings.h"
#include "third_party/blink/renderer/core/frame/visual_viewport.h"
#include "third_party/blink/renderer/core/html/html_body_element.h"
#include "third_party/blink/renderer/core/html/html_div_element.h"
#include "third_party/blink/renderer/core/html/html_head_element.h"
#include "third_party/blink/renderer/core/html/html_html_element.h"
#include "third_party/blink/renderer/core/html/html_image_element.h"
#include "third_party/blink/renderer/core/html/html_meta_element.h"
#include "third_party/blink/renderer/core/html/html_slot_element.h"
#include "third_party/blink/renderer/core/html_names.h"
#include "third_party/blink/renderer/core/layout/layout_object.h"
#include "third_party/blink/renderer/core/loader/document_loader.h"
#include "third_party/blink/renderer/core/loader/frame_loader.h"
#include "third_party/blink/renderer/core/loader/resource/image_resource.h"
#include "third_party/blink/renderer/core/page/chrome_client.h"
#include "third_party/blink/renderer/core/page/page.h"
#include "third_party/blink/renderer/core/paint/paint_layer_scrollable_area.h"
#include "third_party/blink/renderer/platform/bindings/exception_state.h"
#include "third_party/blink/renderer/platform/heap/garbage_collected.h"
#include "third_party/blink/renderer/platform/instrumentation/use_counter.h"
#include "third_party/blink/renderer/platform/wtf/casting.h"
#include "third_party/blink/renderer/platform/wtf/text/string_builder.h"
namespace blink {
class ImageEventListener : public NativeEventListener {
 public:
  ImageEventListener(ImageDocument* document) : doc_(document) {}
  bool Matches(const EventListener& other) const override;
  void Invoke(ExecutionContext*, Event*) override;
  void Trace(Visitor* visitor) const override {
    visitor->Trace(doc_);
    NativeEventListener::Trace(visitor);
  }
  bool IsImageEventListener() const override { return true; }
 private:
  Member<ImageDocument> doc_;
};
template <>
struct DowncastTraits<ImageEventListener> {
  static bool AllowFrom(const EventListener& event_listener) {
    const NativeEventListener* native_event_listener =
        DynamicTo<NativeEventListener>(event_listener);
    return native_event_listener &&
           native_event_listener->IsImageEventListener();
  }
};
class ImageDocumentParser : public RawDataDocumentParser {
 public:
  ImageDocumentParser(ImageDocument* document)
      : RawDataDocumentParser(document),
        world_(document->GetExecutionContext()->GetCurrentWorld()) {}
  ImageDocument* GetDocument() const {
    return To<ImageDocument>(RawDataDocumentParser::GetDocument());
  }
  void Trace(Visitor* visitor) const override {
    visitor->Trace(image_resource_);
    RawDataDocumentParser::Trace(visitor);
  }
 private:
  void AppendBytes(const char*, size_t) override;
  void Finish() override;
  Member<ImageResource> image_resource_;
  const scoped_refptr<const DOMWrapperWorld> world_;
};
// --------
static String ImageTitle(const String& filename, const gfx::Size& size) {
  StringBuilder result;
  result.Append(filename);
  result.Append(" (");
  // FIXME: Localize numbers. Safari/OSX shows localized numbers with group
  // separaters. For example, "1,920x1,080".
  result.AppendNumber(size.width());
  result.Append(static_cast<UChar>(0xD7));  // U+00D7 (multiplication sign)
  result.AppendNumber(size.height());
  result.Append(')');
  return result.ToString();
}
void ImageDocumentParser::AppendBytes(const char* data, size_t length) {
  if (!length)
    return;
  if (IsDetached())
    return;
  LocalFrame* frame = GetDocument()->GetFrame();
  Settings* settings = frame->GetSettings();
  bool allow_image = !settings || settings->GetImagesEnabled();
  if (auto* client = frame->GetContentSettingsClient()) {
    allow_image = client->AllowImage(allow_image, GetDocument()->Url());
  }
  if (!allow_image) {
    return;
  }
  if (!image_resource_) {
    ResourceRequest request(GetDocument()->Url());
    request.SetCredentialsMode(network::mojom::CredentialsMode::kOmit);
    image_resource_ = ImageResource::Create(request, world_);
    image_resource_->NotifyStartLoad();
    GetDocument()->CreateDocumentStructure(image_resource_->GetContent());
    if (IsStopped())
      return;
    if (DocumentLoader* loader = GetDocument()->Loader())
      image_resource_->ResponseReceived(loader->GetResponse());
  }
  CHECK_LE(length, std::numeric_limits<unsigned>::max());
  // If decoding has already failed, there's no point in sending additional
  // data to the ImageResource.
  if (image_resource_->GetStatus() != ResourceStatus::kDecodeError)
    image_resource_->AppendData(data, length);
  if (!IsDetached())
    GetDocument()->ImageUpdated();
}
void ImageDocumentParser::Finish() {
  if (!IsStopped() && image_resource_) {
    // TODO(hiroshige): Use ImageResourceContent instead of ImageResource.
    DocumentLoader* loader = GetDocument()->Loader();
    image_resource_->SetResponse(loader->GetResponse());
    image_resource_->Finish(
        loader->GetTiming().ResponseEnd(),
        GetDocument()->GetTaskRunner(TaskType::kInternalLoading).get());
    if (GetDocument()->CachedImage()) {
      GetDocument()->UpdateTitle();
      if (IsDetached())
        return;
      GetDocument()->ImageUpdated();
      GetDocument()->ImageLoaded();
    }
  }
  if (!IsDetached()) {
    GetDocument()->SetReadyState(Document::kInteractive);
    GetDocument()->FinishedParsing();
  }
}
// --------
ImageDocument::ImageDocument(const DocumentInit& initializer)
    : HTMLDocument(initializer, {DocumentClass::kImage}),
      div_element_(nullptr),
      image_element_(nullptr),
      image_size_is_known_(false),
      did_shrink_image_(false),
      should_shrink_image_(ShouldShrinkToFit()),
      image_is_loaded_(false),
      shrink_to_fit_mode_(GetFrame()->GetSettings()->GetViewportEnabled()
                              ? kViewport
                              : kDesktop) {
  SetCompatibilityMode(kNoQuirksMode);
  LockCompatibilityMode();
}
DocumentParser* ImageDocument::CreateParser() {
  return MakeGarbageCollected<ImageDocumentParser>(this);
}
gfx::Size ImageDocument::ImageSize() const {
  DCHECK(image_element_);
  DCHECK(image_element_->CachedImage());
  return image_element_->CachedImage()->IntrinsicSize(
      LayoutObject::ShouldRespectImageOrientation(
          image_element_->GetLayoutObject()));
}
void ImageDocument::CreateDocumentStructure(
    ImageResourceContent* image_content) {
  auto* root_element = MakeGarbageCollected<HTMLHtmlElement>(*this);
  root_element->SetInlineStyleProperty(
      CSSPropertyID::kHeight, 100, CSSPrimitiveValue::UnitType::kPercentage);
  AppendChild(root_element);
  root_element->InsertedByParser();
  if (IsStopped())
    return;  // runScriptsAtDocumentElementAvailable can detach the frame.
  auto* head = MakeGarbageCollected<HTMLHeadElement>(*this);
  auto* meta =
      MakeGarbageCollected<HTMLMetaElement>(*this, CreateElementFlags());
  meta->setAttribute(html_names::kNameAttr, AtomicString("viewport"));
  meta->setAttribute(html_names::kContentAttr,
                     AtomicString("width=device-width, minimum-scale=0.1"));
  head->AppendChild(meta);
  auto* body = MakeGarbageCollected<HTMLBodyElement>(*this);
  body->SetInlineStyleProperty(CSSPropertyID::kMargin, 0.0,
                               CSSPrimitiveValue::UnitType::kPixels);
  body->SetInlineStyleProperty(CSSPropertyID::kHeight, 100.0,
                               CSSPrimitiveValue::UnitType::kPercentage);
  if (ShouldShrinkToFit()) {
    // Display the image prominently centered in the frame.
    body->SetInlineStyleProperty(
        CSSPropertyID::kBackgroundColor,
        *cssvalue::CSSColor::Create(Color::FromRGB(14, 14, 14)));
    // See w3c example on how to center an element:
    // https://www.w3.org/Style/Examples/007/center.en.html
    div_element_ = MakeGarbageCollected<HTMLDivElement>(*this);
    div_element_->SetInlineStyleProperty(CSSPropertyID::kDisplay,
                                         CSSValueID::kFlex);
    div_element_->SetInlineStyleProperty(CSSPropertyID::kFlexDirection,
                                         CSSValueID::kColumn);
    div_element_->SetInlineStyleProperty(CSSPropertyID::kAlignItems,
                                         CSSValueID::kFlexStart);
    div_element_->SetInlineStyleProperty(CSSPropertyID::kMinWidth,
                                         CSSValueID::kMinContent);
    div_element_->SetInlineStyleProperty(
        CSSPropertyID::kHeight, 100.0,
        CSSPrimitiveValue::UnitType::kPercentage);
    div_element_->SetInlineStyleProperty(
        CSSPropertyID::kWidth, 100.0, CSSPrimitiveValue::UnitType::kPercentage);
    HTMLSlotElement* slot = MakeGarbageCollected<HTMLSlotElement>(*this);
    div_element_->AppendChild(slot);
    // Adding a UA shadow root here is because the container <div> should be
    // hidden so that only the <img> element should be visible in <body>,
    // according to the spec:
    // https://html.spec.whatwg.org/C/#read-media
    ShadowRoot& shadow_root = body->EnsureUserAgentShadowRoot();
    shadow_root.AppendChild(div_element_);
  }
  WillInsertBody();
  image_element_ = MakeGarbageCollected<HTMLImageElement>(*this);
  UpdateImageStyle();
  image_element_->StartLoadingImageDocument(image_content);
  body->AppendChild(image_element_.Get());
  if (ShouldShrinkToFit()) {
    // Add event listeners
    auto* listener = MakeGarbageCollected<ImageEventListener>(this);
    if (LocalDOMWindow* dom_window = domWindow())
      dom_window->addEventListener(event_type_names::kResize, listener, false);
    if (shrink_to_fit_mode_ == kDesktop) {
      image_element_->addEventListener(event_type_names::kClick, listener,
                                       false);
    } else if (shrink_to_fit_mode_ == kViewport) {
      image_element_->addEventListener(event_type_names::kTouchend, listener,
                                       false);
      image_element_->addEventListener(event_type_names::kTouchcancel, listener,
                                       false);
    }
  }
  root_element->AppendChild(head);
  root_element->AppendChild(body);
  if (IsStopped())
    image_element_ = nullptr;
}
void ImageDocument::UpdateTitle() {
  // Report the natural image size in the page title, regardless of zoom
  // level.  At a zoom level of 1 the image is guaranteed to have an integer
  // size.
  gfx::Size size = ImageSize();
  if (!size.width())
    return;
  // Compute the title, we use the decoded filename of the resource, falling
  // back on the (decoded) hostname if there is no path.
  String file_name = DecodeURLEscapeSequences(Url().LastPathComponent(),
                                              DecodeURLMode::kUTF8OrIsomorphic);
  if (file_name.empty())
    file_name = Url().Host();
  setTitle(ImageTitle(file_name, size));
}
float ImageDocument::Scale() const {
  DCHECK_EQ(shrink_to_fit_mode_, kDesktop);
  if (!image_element_ || image_element_->GetDocument() != this)
    return 1.0f;
  LocalFrameView* view = GetFrame()->View();
  if (!view)
    return 1.0f;
  gfx::Size image_size = ImageSize();
  if (image_size.IsEmpty())
    return 1.0f;
  // We want to pretend the viewport is larger when the user has zoomed the
  // page in (but not when the zoom is coming from device scale).
  const float viewport_zoom =
      view->GetChromeClient()->WindowToViewportScalar(GetFrame(), 1.f);
  float width_scale = view->Width() / (viewport_zoom * image_size.width());
  float height_scale = view->Height() / (viewport_zoom * image_size.height());
  return std::min(width_scale, height_scale);
}
void ImageDocument::ResizeImageToFit() {
  DCHECK_EQ(shrink_to_fit_mode_, kDesktop);
  if (!image_element_ || image_element_->GetDocument() != this)
    return;
  gfx::Size image_size = gfx::ScaleToFlooredSize(ImageSize(), Scale());
  image_element_->setWidth(image_size.width());
  image_element_->setHeight(image_size.height());
  UpdateImageStyle();
}
void ImageDocument::ImageClicked(int x, int y) {
  DCHECK_EQ(shrink_to_fit_mode_, kDesktop);
  if (!image_size_is_known_ || ImageFitsInWindow())
    return;
  should_shrink_image_ = !should_shrink_image_;
  if (should_shrink_image_) {
    WindowSizeChanged();
  } else {
    // Adjust the coordinates to account for the fact that the image was
    // centered on the screen.
    float image_x = x - image_element_->OffsetLeft();
    float image_y = y - image_element_->OffsetTop();
    RestoreImageSize();
    UpdateStyleAndLayout(DocumentUpdateReason::kInput);
    double scale = Scale();
    double device_scale_factor =
        GetFrame()->View()->GetChromeClient()->WindowToViewportScalar(
            GetFrame(), 1.f);
    float scroll_x = (image_x * device_scale_factor) / scale -
                     static_cast<float>(GetFrame()->View()->Width()) / 2;
    float scroll_y = (image_y * device_scale_factor) / scale -
                     static_cast<float>(GetFrame()->View()->Height()) / 2;
    GetFrame()->View()->LayoutViewport()->SetScrollOffset(
        ScrollOffset(scroll_x, scroll_y),
        mojom::blink::ScrollType::kProgrammatic);
  }
}
void ImageDocument::ImageLoaded() {
  image_is_loaded_ = true;
  UpdateImageStyle();
}
ImageDocument::MouseCursorMode ImageDocument::ComputeMouseCursorMode() const {
  if (!image_is_loaded_)
    return kDefault;
  if (shrink_to_fit_mode_ != kDesktop || !ShouldShrinkToFit())
    return kDefault;
  if (ImageFitsInWindow())
    return kDefault;
  return should_shrink_image_ ? kZoomIn : kZoomOut;
}
void ImageDocument::UpdateImageStyle() {
  StringBuilder image_style;
  image_style.Append("display: block;");
  image_style.Append("-webkit-user-select: none;");
  if (ShouldShrinkToFit()) {
    if (shrink_to_fit_mode_ == kViewport)
      image_style.Append("max-width: 100%;");
    image_style.Append("margin: auto;");
  }
  MouseCursorMode cursor_mode = ComputeMouseCursorMode();
  if (cursor_mode == kZoomIn)
    image_style.Append("cursor: zoom-in;");
  else if (cursor_mode == kZoomOut)
    image_style.Append("cursor: zoom-out;");
  if (GetFrame()->IsOutermostMainFrame()) {
    if (image_is_loaded_) {
      image_style.Append("background-color: hsl(0, 0%, 90%);");
      DCHECK(image_element_);
      DCHECK(image_element_->CachedImage());
      if (!image_element_->CachedImage()->IsAnimatedImage()) {
        image_style.Append("transition: background-color 300ms;");
      }
    } else if (image_size_is_known_) {
      image_style.Append("background-color: hsl(0, 0%, 25%);");
    }
  }
  image_element_->setAttribute(html_names::kStyleAttr,
                               image_style.ToAtomicString());
}
void ImageDocument::ImageUpdated() {
  DCHECK(image_element_);
  if (image_size_is_known_)
    return;
  UpdateStyleAndLayoutTree();
  if (!image_element_->CachedImage() || ImageSize().IsEmpty())
    return;
  image_size_is_known_ = true;
  UpdateImageStyle();
  if (ShouldShrinkToFit()) {
    // Force resizing of the image
    WindowSizeChanged();
  }
}
void ImageDocument::RestoreImageSize() {
  DCHECK_EQ(shrink_to_fit_mode_, kDesktop);
  if (!image_element_ || !image_size_is_known_ ||
      image_element_->GetDocument() != this)
    return;
  gfx::Size image_size = ImageSize();
  image_element_->setWidth(image_size.width());
  image_element_->setHeight(image_size.height());
  UpdateImageStyle();
  did_shrink_image_ = false;
}
bool ImageDocument::ImageFitsInWindow() const {
  DCHECK_EQ(shrink_to_fit_mode_, kDesktop);
  return Scale() >= 1;
}
int ImageDocument::CalculateDivWidth() {
  // Zooming in and out of an image being displayed within a viewport is done
  // by changing the page scale factor of the page instead of changing the
  // size of the image.  The size of the image is set so that:
  // * Images wider than the viewport take the full width of the screen.
  // * Images taller than the viewport are initially aligned with the top of
  //   of the frame.
  // * Images smaller in either dimension are centered along that axis.
  int viewport_width =
      GetFrame()->GetPage()->GetVisualViewport().Size().width() /
      GetFrame()->PageZoomFactor();
  // For huge images, minimum-scale=0.1 is still too big on small screens.
  // Set the <div> width so that the image will shrink to fit the width of the
  // screen when the scale is minimum.
  int max_width = std::min(ImageSize().width(), viewport_width * 10);
  return std::max(viewport_width, max_width);
}
void ImageDocument::WindowSizeChanged() {
  if (!image_element_ || !image_size_is_known_ ||
      image_element_->GetDocument() != this)
    return;
  if (shrink_to_fit_mode_ == kViewport) {
    int div_width = CalculateDivWidth();
    div_element_->SetInlineStyleProperty(CSSPropertyID::kWidth, div_width,
                                         CSSPrimitiveValue::UnitType::kPixels);
    // Explicitly set the height of the <div> containing the <img> so that it
    // can display the full image without shrinking it, allowing a full-width
    // reading mode for normal-width-huge-height images. Use the LayoutSize
    // for height rather than viewport since that doesn't change based on the
    // URL bar coming in and out - thus preventing the image from jumping
    // around. i.e. The div should fill the viewport when minimally zoomed and
    // the URL bar is showing, but won't fill the new space when the URL bar
    // hides.
    gfx::Size layout_size = View()->GetLayoutSize();
    float aspect_ratio =
        static_cast<float>(layout_size.width()) / layout_size.height();
    int div_height = std::max(ImageSize().height(),
                              static_cast<int>(div_width / aspect_ratio));
    div_element_->SetInlineStyleProperty(CSSPropertyID::kHeight, div_height,
                                         CSSPrimitiveValue::UnitType::kPixels);
    return;
  }
  bool fits_in_window = ImageFitsInWindow();
  // If the image has been explicitly zoomed in, restore the cursor if the image
  // fits and set it to a zoom out cursor if the image doesn't fit
  if (!should_shrink_image_) {
    UpdateImageStyle();
    return;
  }
  if (did_shrink_image_) {
    // If the window has been resized so that the image fits, restore the image
    // size otherwise update the restored image size.
    if (fits_in_window)
      RestoreImageSize();
    else
      ResizeImageToFit();
  } else {
    // If the image isn't resized but needs to be, then resize it.
    if (!fits_in_window) {
      ResizeImageToFit();
      did_shrink_image_ = true;
    }
  }
}
ImageResourceContent* ImageDocument::CachedImage() {
  if (!image_element_)
    return nullptr;
  return image_element_->CachedImage();
}
bool ImageDocument::ShouldShrinkToFit() const {
  // WebView automatically resizes to match the contents, causing an infinite
  // loop as the contents then resize to match the window. To prevent this,
  // disallow images from shrinking to fit for WebViews.
  bool is_wrap_content_web_view =
      GetPage() ? GetPage()->GetSettings().GetForceZeroLayoutHeight() : false;
  return GetFrame()->IsOutermostMainFrame() && !is_wrap_content_web_view;
}
void ImageDocument::Trace(Visitor* visitor) const {
  visitor->Trace(div_element_);
  visitor->Trace(image_element_);
  HTMLDocument::Trace(visitor);
}
// --------
void ImageEventListener::Invoke(ExecutionContext*, Event* event) {
  auto* mouse_event = DynamicTo<MouseEvent>(event);
  if (event->type() == event_type_names::kResize) {
    doc_->WindowSizeChanged();
  } else if (event->type() == event_type_names::kClick && mouse_event) {
    doc_->ImageClicked(mouse_event->x(), mouse_event->y());
  } else if ((event->type() == event_type_names::kTouchend ||
              event->type() == event_type_names::kTouchcancel) &&
             IsA<TouchEvent>(event)) {
    doc_->UpdateImageStyle();
  }
}
bool ImageEventListener::Matches(const EventListener& listener) const {
  if (const ImageEventListener* image_event_listener =
          DynamicTo<ImageEventListener>(listener)) {
    return doc_ == image_event_listener->doc_;
  }
  return false;
}
}  // namespace blink
<div class="col-md-3 s-lg-tabs-side pad-bottom-med">
                <div class="icon-sidebar">
                    <a href="https://www.nwtc.edu/student-experience/library" aria-label="NWTC Libraries homepage"><i class="fa fa-home" aria-hidden="true"></i></a>
                    <a href="https://nwtc.libanswers.com/faq/212590" aria-label="Library Hours"><i class="fa fa-clock-o" aria-hidden="true"></i></a>
                    <a href="mailto:ask.library@nwtc.edu" aria-label="Email NWTC Libraries"><i class="fa fa-envelope-o" aria-hidden="true"></i></a> 
                    <a href="tel:(920) 498-5493" aria-label="Call NWTC Libraries"><i class="fa fa-phone" aria-hidden="true"></i></a>
                    <a href="https://www.facebook.com/NWTC.Library/" aria-label="Facebook"><i class="fa fa-facebook" aria-hidden="true"></i></a>
                    <a href="https://www.instagram.com/nwtclibrary/" aria-label="Instagram"><i class="fa fa-instagram" aria-hidden="true"></i></a>
                    <a href="https://www.youtube.com/channel/UCgZ6qotMxC3UbxeHNCjVXnA/" aria-label="YouTube"><i class="fa fa-youtube" aria-hidden="true"></i></a>
                </div>
        <div id="s-lg-tabs-container" class="container s-lib-side-borders pad-top-med">
            <div id="s-lg-guide-tabs">
                <div class="row">
                    <div class="col-md-3 s-lg-tabs-side pad-bottom-med">
                        <ul class="nav nav-pills nav-stacked"><li role="menuitem" class=""><a title="" class="" href="https://nwtc.libguides.com/c.php?g=29261&amp;p=182452"><span>Home</span></a></li><li class="active"><a title="" class=" active" href="https://nwtc.libguides.com/citations/APA7"><span>APA Style: 7th edition</span></a><ul class="list-group s-lg-boxnav"><li class="list-group-item"><a href="#s-lg-box-15820984">APA Quick Links</a></li><li class="list-group-item"><a href="#s-lg-box-22759063">Basic Overview of APA Style</a></li><li class="list-group-item"><a href="#s-lg-box-29511854">Handouts &amp; Guides from the Official  APA Website</a></li><li class="list-group-item"><a href="#s-lg-box-24847788">Paper Format Overview</a></li><li class="list-group-item"><a href="#s-lg-box-29302529">Student Paper Setup Guide</a></li><li class="list-group-item"><a href="#s-lg-box-29302822">Sample APA Papers</a></li><li class="list-group-item"><a href="#s-lg-box-22995045">Formatting the Title Page </a></li><li class="list-group-item"><a href="#s-lg-box-28990452">Formatting the (Optional) Abstract Page</a></li><li class="list-group-item"><a href="#s-lg-box-29280349">Formatting the References Page</a></li><li class="list-group-item"><a href="#s-lg-box-27521460">Formatting a PowerPoint Presentation in APA Style</a></li><li class="list-group-item"><a href="#s-lg-box-29043729">Formatting an Annotated Bibliography</a></li><li class="list-group-item"><a href="#s-lg-box-22759078">Citations in the Paper's Body</a></li><li class="list-group-item"><a href="#s-lg-box-31040955">Authors' Names in APA In-Text Citations</a></li><li class="list-group-item"><a href="#s-lg-box-31041774">Direct Quoting and Paraphrasing</a></li><li class="list-group-item"><a href="#s-lg-box-28661952">Capitalization</a></li><li class="list-group-item"><a href="#s-lg-box-22766629">No page numbers? </a></li><li class="list-group-item"><a href="#s-lg-box-31045963">Authors' Names on the References Page</a></li><li class="list-group-item"><a href="#s-lg-box-22770224">Journal Articles</a></li><li class="list-group-item"><a href="#s-lg-box-22770252">Magazine Articles</a></li><li class="list-group-item"><a href="#s-lg-box-24565494">Newspaper Articles</a></li><li class="list-group-item"><a href="#s-lg-box-31488680">Blog Post</a></li><li class="list-group-item"><a href="#s-lg-box-22762598">Webpage: Individual Author</a></li><li class="list-group-item"><a href="#s-lg-box-22762581">Webpage: Government Agency as Group Author</a></li><li class="list-group-item"><a href="#s-lg-box-28991525">Webpage: Organization as Group Author</a></li><li class="list-group-item"><a href="#s-lg-box-22770292">Books/E-Books and Book Chapters</a></li><li class="list-group-item"><a href="#s-lg-box-24883932">Published Dissertation or Thesis</a></li><li class="list-group-item"><a href="#s-lg-box-22770371">Online Videos</a></li><li class="list-group-item"><a href="#s-lg-box-22770851">PowerPoint Slides</a></li><li class="list-group-item"><a href="#s-lg-box-22770982">Interviews</a></li><li class="list-group-item"><a href="#s-lg-box-28996741">Legal Sources (Court Cases, Legislation, Statutes)</a></li><li class="list-group-item"><a href="#s-lg-box-27819773">Oral Teachings of Indigenous Elders and Knowledge Keepers</a></li><li class="list-group-item"><a href="#s-lg-box-30561708">ChatGPT or Other Generative AI</a></li></ul><ul class="s-lg-subtab-ul nav nav-pills nav-stacked"><li class=""><a title="" href="https://nwtc.libguides.com/c.php?g=29261&amp;p=9600724" style="" "="">APA Formatting Tips </a></li></ul></li><li role="menuitem" class=""><a title="" class="" href="https://nwtc.libguides.com/citations/MLA"><span>MLA Style</span></a></li><li role="menuitem" class=""><a title="" class="" href="https://nwtc.libguides.com/c.php?g=29261&amp;p=182457"><span>Citation Books in the Library</span></a></li><li role="menuitem" class=""><a title="" class="" href="https://nwtc.libguides.com/c.php?g=29261&amp;p=182458"><span>Citation Formatting in Word &amp; Google Docs</span></a></li><li role="menuitem" class=""><a title="" class="" href="https://nwtc.libguides.com/c.php?g=29261&amp;p=818891"><span>Quoting and Paraphrasing</span></a></li><li role="menuitem" class=""><a title="" class="" href="https://nwtc.libguides.com/c.php?g=29261&amp;p=7423033"><span>Citation Managers</span></a></li>
</ul>
<div class="s-lg-row margin-top-med"><div id="s-lg-col-0"><div class="s-lg-col-boxes"></div></div></div>
                    </div>
                    <div class="col-md-9">
                        <div class="s-lg-tab-content">
                            <div class="tab-pane active">
                               <div class="row s-lg-row">

     <div id="s-lg-col-1" class="col-md-12">
          <div class="s-lg-col-boxes">
               <div id="s-lg-box-wrapper-35843799" class="s-lg-box-wrapper-35843799">
					<div id="s-lg-box-15820984-container" class="s-lib-box-container">
						<div id="s-lg-box-15820984" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">APA Quick Links
                                </h2>
							<div id="s-lg-box-collapse-15820984">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									<div id="s-lib-ctabs-15820984" class="s-lib-jqtabs">
    <ul class="nav nav-tabs" role="tablist">
                    <li role="presentation" class="active"><a href="#s-lib-ctab-15820984-0" role="tab" data-toggle="tab">APA Guides</a></li>
                    <li role="presentation"><a href="#s-lib-ctab-15820984-1" role="tab" data-toggle="tab">Quick How-To Videos</a></li>
                    <li role="presentation"><a href="#s-lib-ctab-15820984-2" role="tab" data-toggle="tab">Citation Generators</a></li>
                    <li role="presentation"><a href="#s-lib-ctab-15820984-3" role="tab" data-toggle="tab">Quoting &amp; Paraphrasing</a></li>
                    <li role="presentation"><a href="#s-lib-ctab-15820984-4" role="tab" data-toggle="tab">Plagiarism</a></li>
            </ul>
    <div class="tab-content">
                    <div role="tabpanel" class="tab-pane active" id="s-lib-ctab-15820984-0">
                <div class=""><ul id="s-lg-link-list-8308295" class="s-lg-link-list s-lg-link-list-2"><li class="s-lg-tn-li clearfix"><div id="s-lg-content-41791157" class=""><span><a href="https://apastyle.apa.org/style-grammar-guidelines/" target="_blank" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '41791157',_st_inc_return: this});"><div id="s-lg-tn-41791157" class="s-lg-tn pad-right-sm"><img loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" class="pull-left margin-right-sm lg-tn" width="51" height="24" alt="APA icon"></div>APA Style &amp; Grammar Guidelines</a></span><div id="s-lg-link-desc-41791157" class="s-lg-link-desc">Official companion to the APA Manual</div></div></li><li class="s-lg-tn-li clearfix"><div id="s-lg-content-62598779" class=""><span><a href="https://owl.excelsior.edu/citation-and-documentation/apa-style/" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '62598779',_st_inc_return: this});"><div id="s-lg-tn-62598779" class="s-lg-tn pad-right-sm"><img loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" class="pull-left margin-right-sm lg-tn" width="51" height="24"></div>APA Citation &amp; Documentation Guide</a></span><div id="s-lg-link-desc-62598779" class="s-lg-link-desc">Excelsior College Online Writing Lab</div></div></li></ul></div>
            </div>
                    <div role="tabpanel" class="tab-pane " id="s-lib-ctab-15820984-1">
                <div class=""><ul id="s-lg-link-list-9070712" class="s-lg-link-list s-lg-link-list-2"><li class="s-lg-tn-li clearfix"><div id="s-lg-content-72095448" class=""><span><a href="https://www.youtube.com/watch?v=BWE-gQ_OSHI" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '72095448',_st_inc_return: this});"><div id="s-lg-tn-72095448" class="s-lg-tn pad-right-sm"><img loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" class="pull-left margin-right-sm lg-tn" width="51" height="24"></div>Overview of APA Style in 4 Minutes (video)</a></span></div></li><li class="s-lg-tn-li clearfix"><div id="s-lg-content-70973095" class=""><span><a href="https://youtu.be/4usdRatofC8" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '70973095',_st_inc_return: this});"><div id="s-lg-tn-70973095" class="s-lg-tn pad-right-sm"><img loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" class="pull-left margin-right-sm lg-tn" width="51" height="24"></div>APA Paper Format in 2 Minutes (video)</a></span></div></li><li class="s-lg-tn-li clearfix"><div id="s-lg-content-70974661" class=""><span><a href="https://youtu.be/JYBXhL_xMoo" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '70974661',_st_inc_return: this});"><div id="s-lg-tn-70974661" class="s-lg-tn pad-right-sm"><img loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" class="pull-left margin-right-sm lg-tn" width="51" height="24"></div>APA Title Page Format in 2 Minutes (video)</a></span></div></li><li class="s-lg-tn-li clearfix"><div id="s-lg-content-70974676" class=""><span><a href="https://youtu.be/OB2jA5aFnQ0" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '70974676',_st_inc_return: this});"><div id="s-lg-tn-70974676" class="s-lg-tn pad-right-sm"><img loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" class="pull-left margin-right-sm lg-tn" width="51" height="24"></div>APA References Page Format in 2 Minutes (video)</a></span></div></li><li class="s-lg-tn-li clearfix"><div id="s-lg-content-70976905" class=""><span><a href="https://youtu.be/OpSPiES57s4" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '70976905',_st_inc_return: this});"><div id="s-lg-tn-70976905" class="s-lg-tn pad-right-sm"><img loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" class="pull-left margin-right-sm lg-tn" width="51" height="24"></div>APA In-Text Citation Format in 3 Minutes (video)</a></span></div></li><li class="s-lg-tn-li clearfix"><div id="s-lg-content-73430882" class=""><span><a href="https://youtu.be/IHUZ0InywrQ" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '73430882',_st_inc_return: this});"><div id="s-lg-tn-73430882" class="s-lg-tn pad-right-sm"><img loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" class="pull-left margin-right-sm lg-tn" width="51" height="24"></div>Citing a Journal Article in APA Format (3 minute video)</a></span></div></li></ul></div>
            </div>
                    <div role="tabpanel" class="tab-pane " id="s-lib-ctab-15820984-2">
                <div class=""><ul id="s-lg-link-list-8308365" class="s-lg-link-list s-lg-link-list-2"><li class="s-lg-tn-li clearfix"><div id="s-lg-content-68121893" class=""><span><a href="https://www.scribbr.com/apa-citation-generator/" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68121893',_st_inc_return: this});"><div id="s-lg-tn-68121893" class="s-lg-tn pad-right-sm"><img loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" class="pull-left margin-right-sm lg-tn" height="24"></div>Scribbr APA Citation Generator</a></span></div></li><li class="s-lg-tn-li clearfix"><div id="s-lg-content-20877506" class=""><span><a href="https://www.citationmachine.net/apa" target="_blank" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '20877506',_st_inc_return: this});"><div id="s-lg-tn-20877506" class="s-lg-tn pad-right-sm"><img loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" class="pull-left margin-right-sm lg-tn" width="51" height="24" alt="APA icon"></div>Citation Machine</a></span><div id="s-lg-link-desc-20877506" class="s-lg-link-desc">Generates APA citations</div></div></li></ul></div>
            </div>
                    <div role="tabpanel" class="tab-pane " id="s-lib-ctab-15820984-3">
                <div class=""><ul id="s-lg-link-list-2983540" class="s-lg-link-list s-lg-link-list-2"><li class=""><div id="s-lg-content-34568925" class=""><span><a href="https://writingcenter.unc.edu/handouts/quotations/" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '34568925',_st_inc_return: this});">Quotations Handout</a></span><div id="s-lg-link-desc-34568925" class="s-lg-link-desc">UNC-Chapel Hill Writing Center</div></div></li><li class=""><div id="s-lg-content-34568926" class=""><span><a href="https://www.writing.wisc.edu/Handbook/QuotingSources.html" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '34568926',_st_inc_return: this});">Quoting and Paraphrasing</a></span><div id="s-lg-link-desc-34568926" class="s-lg-link-desc">UW-Madison Writing Center</div></div></li><li class=""><div id="s-lg-content-68121713" class=""><span><a href="https://apastyle.apa.org/style-grammar-guidelines/citations/quotations" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68121713',_st_inc_return: this});">Direct Quotations</a></span><div id="s-lg-link-desc-68121713" class="s-lg-link-desc">Official APA Style &amp; Grammar Guidlelines</div></div></li><li class=""><div id="s-lg-content-68121722" class=""><span><a href="https://apastyle.apa.org/style-grammar-guidelines/citations/paraphrasing" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68121722',_st_inc_return: this});">Paraphrasing</a></span><div id="s-lg-link-desc-68121722" class="s-lg-link-desc">Official APA Style &amp; Grammar Guidelines</div></div></li><li class=""><div id="s-lg-content-68121743" class=""><span><a href="https://apastyle.apa.org/instructional-aids/paraphrasing-citation-activities.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68121743',_st_inc_return: this});">Paraphrasing and Citation Activities</a></span><div id="s-lg-link-desc-68121743" class="s-lg-link-desc">Official APA Style &amp; Grammar Guidelines</div></div></li></ul></div>
            </div>
                    <div role="tabpanel" class="tab-pane " id="s-lib-ctab-15820984-4">
                <div class=""><ul id="s-lg-link-list-2983538" class="s-lg-link-list s-lg-link-list-2"><li class=""><div id="s-lg-content-34568923" class=""><span><a href="https://nwtc.libguides.com/plagiarism" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '34568923',_st_inc_return: this});">Plagiarism Overview Library Guide</a></span></div></li><li class=""><div id="s-lg-content-68121768" class=""><span><a href="https://apastyle.apa.org/instructional-aids/avoiding-plagiarism.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68121768',_st_inc_return: this});">Avoiding Plagiarism Guide</a></span><div id="s-lg-link-desc-68121768" class="s-lg-link-desc">Official APA Style &amp; Grammar Guidelines</div></div></li><li class=""><div id="s-lg-content-34568924" class=""><span><a href="https://www.wisc-online.com/objects/ViewObject.aspx?ID=TRG2803" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '34568924',_st_inc_return: this});">Interactive Plagiarism Tutorial</a></span><div id="s-lg-link-desc-34568924" class="s-lg-link-desc">Created by Dave Wehmeyer, NWTC Instructor</div></div></li><li class=""><div id="s-lg-content-69447831" class=""><span><a href="https://owl.excelsior.edu/plagiarism/" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '69447831',_st_inc_return: this});">Avoiding Plagiarism Tutorial</a></span><div id="s-lg-link-desc-69447831" class="s-lg-link-desc">Topics include learning to recognize <a href="https://owl.excelsior.edu/plagiarism/plagiarism-what-is-it/plagiarism-types-of-plagiarism/">different kinds of plagiarism</a> and <a href="https://owl.excelsior.edu/plagiarism/plagiarism-how-to-avoid-it/">how to avoid plagiarism</a> by citing sources correctly and writing good paraphrases. (Excelsior University Online Writing Lab)</div></div></li></ul></div>
            </div>
            </div>
</div>
<script>
    // Every time a tab is switched, remove the tabindex for accessibility and keyboard nav
    jQuery('#s-lib-ctabs-15820984 ul.nav-tabs li a').on('shown.bs.tab', function (e) {
        jQuery('#s-lib-ctabs-15820984 ul.nav-tabs li a').removeAttr('tabindex');
    });

    // Support direct linking to tabs
    jQuery(function() {
        var hash = window.location.hash;

        // Don't try to acccess garbage parameters
        if (hash.length === 0 || jQuery('.s-lib-jqtabs a[href="' + hash + '"]').get(0) === undefined) {
            return;
        }

        // Show the tab
        jQuery('.s-lib-jqtabs a[href="' + hash + '"]').tab('show');
    });
</script>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26705920" class="s-lg-box-wrapper-26705920">
					<div id="s-lg-box-22759063-container" class="s-lib-box-container">
						<div id="s-lg-box-22759063" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Basic Overview of APA Style
                                </h2>
							<div id="s-lg-box-collapse-22759063">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-51556559" class="  clearfix">
				<p><span style="font-size:14px;"><img alt="APA" loading="lazy" src="//libapps.s3.amazonaws.com/customers/1759/images/APA.jpg" style="width: 63px; height: 30px;">&nbsp;</span><span style="font-size:14px;"></span></p>

<p>Every time you <a href="https://apastyle.apa.org/style-grammar-guidelines/citations/quotations">quote </a>or <a href="https://apastyle.apa.org/style-grammar-guidelines/citations/paraphrasing">paraphrase </a>someone else’s work, you must indicate:</p>

<ul>
	<li>who wrote the work</li>
	<li>what is it called</li>
	<li>and where we (the reader) can find a copy.</li>
</ul>

<p>You give us this information in two places:</p>

<ol>
	<li>In the <a href="https://nwtc.libguides.com/citations/apa7#s-lg-box-22759078">body of the paper</a>, specifically the paragraph where you are quoting or paraphrasing. This is often called an&nbsp;In Text or Parenthetical Citation because you will put brief information about the work in parentheses. Check out our guidelines and examples in the left-hand column, as well as these resources from the <a href="https://apastyle.apa.org/style-grammar-guidelines">official APA Style &amp; Grammar Guidelines</a>:

	<ul>
		<li><a href="https://apastyle.apa.org/style-grammar-guidelines/citations/basic-principles/author-date">Author-Date Citation System</a></li>
		<li><a href="https://apastyle.apa.org/style-grammar-guidelines/citations/basic-principles/parenthetical-versus-narrative">Parenthetical versus&nbsp;Narrative In-Text Citation Options</a></li>
	</ul>
	</li>
	<li>In the <a href="https://nwtc.libguides.com/citations/apa7#s-lg-box-29280349">References page</a> at the end of the paper. This is where you put all of the information we need to find a copy of the works you used in your paper. Check out our guidelines and examples in the left-hand column, as well as these resources from the <a href="https://apastyle.apa.org/style-grammar-guidelines">official APA Style &amp; Grammar Guidelines</a>:
	<ul>
		<li><a href="https://apastyle.apa.org/instructional-aids/creating-reference-list.pdf">Creating an APA Style Reference&nbsp;List Guide</a> (2-page handout)</li>
		<li><a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format/student-annotated.pdf">Sample Annotated Student Paper</a>&nbsp;(7-page handout)</li>
	</ul>
	</li>
</ol>

		   </div><div class=""><ul id="s-lg-link-list-73548052" class="s-lg-link-list s-lg-link-list-4"><li class=" pad-bottom-sm"><div id="s-lg-content-70998935" class=""><span><a href="https://nwtc.libguides.com/ld.php?content_id=70998935" rel="nofollow" target="_blank" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '5',_st_content_id: '70998935',_st_inc_return: this});"><i class="s-lg-file-icon fa fa-file-pdf-o" style="font-size:1.5em; margin-right:5px;"></i>APA Format Quick Guide</a></span></div></li></ul></div>
        <div id="s-lg-content-72095476" class="s-lg-widget "><iframe width="560" height="315" src="https://www.youtube.com/embed/BWE-gQ_OSHI?enablejsapi=1&amp;origin=https%3A%2F%2Fnwtc.libguides.com" title="Overview of APA Style" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" data-gtm-yt-inspected-2060666_59="true" id="325088087" data-gtm-yt-inspected-8="true"></iframe>
        </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-34723458" class="s-lg-box-wrapper-34723458">
					<div id="s-lg-box-29511854-container" class="s-lib-box-container">
						<div id="s-lg-box-29511854" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Handouts &amp; Guides from the Official  APA Website
                                </h2>
							<div id="s-lg-box-collapse-29511854">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-68762047" class="  clearfix">
				<p>The&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines">official APA Style &amp; Grammar Guidelines</a>&nbsp;website has a collection of <a href="https://apastyle.apa.org/instructional-aids/handouts-guides">Handouts and Guides</a>, which include:</p>

		   </div><div class=""><ul id="s-lg-link-list-71271586" class="s-lg-link-list s-lg-link-list-2"><li class=""><div id="s-lg-content-68762110" class=""><span><a href="https://apastyle.apa.org/instructional-aids/student-paper-setup-guide.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68762110',_st_inc_return: this});">Student Paper Setup Guide</a></span><div id="s-lg-link-desc-68762110" class="s-lg-link-desc">Annotated diagrams illustrate how to set up the major sections of a student paper: the title page, body of the paper, and reference list.  Short handout version of this one-hour <a href="https://youtu.be/Ae6mQBUVqVE">A Step By Step Guide for APA Style Student Papers webinar</a>.</div></div></li><li class=""><div id="s-lg-content-68762157" class=""><span><a href="https://apastyle.apa.org/instructional-aids/concise-guide-formatting-checklist.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68762157',_st_inc_return: this});">Student Paper Checklist</a></span></div></li><li class=""><div id="s-lg-content-68762118" class=""><span><a href="https://apastyle.apa.org/instructional-aids/student-title-page-guide.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68762118',_st_inc_return: this});">Student Title Page Guide</a></span></div></li><li class=""><div id="s-lg-content-68762167" class=""><span><a href="https://apastyle.apa.org/instructional-aids/in-text-citation-checklist.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68762167',_st_inc_return: this});">In-Text Citation Checklist</a></span></div></li><li class=""><div id="s-lg-content-68762549" class=""><span><a href="https://apastyle.apa.org/instructional-aids/creating-reference-list.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68762549',_st_inc_return: this});">Guide to Creating an APA Style Reference List</a></span></div></li><li class=""><div id="s-lg-content-68762095" class=""><span><a href="https://apastyle.apa.org/instructional-aids/reference-examples.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68762095',_st_inc_return: this});">Common Reference Examples Guide</a></span></div></li><li class=""><div id="s-lg-content-68762081" class=""><span><a href="https://apastyle.apa.org/instructional-aids/reference-guide.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68762081',_st_inc_return: this});">Reference Guide for Journal Articles, Books, and Edited Book Chapters</a></span></div></li><li class=""><div id="s-lg-content-68762370" class=""><span><a href="https://apastyle.apa.org/instructional-aids/journal-article-reference-checklist.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68762370',_st_inc_return: this});">Journal Article Reference Checklist</a></span></div></li></ul></div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-29175841" class="s-lg-box-wrapper-29175841">
					<div id="s-lg-box-24847788-container" class="s-lib-box-container">
						<div id="s-lg-box-24847788" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Paper Format Overview
                                </h2>
							<div id="s-lg-box-collapse-24847788">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-57032559" class="  clearfix">
				<p>The official <a href="https://apastyle.apa.org/style-grammar-guidelines">APA Style &amp; Grammar Guidelines</a>&nbsp;includes a section on <a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format">Paper Format</a>, with information on:</p>

<ul>
	<li><a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format/page-header" style="background-color: rgb(255, 255, 255);">Page Headers</a>:&nbsp;For student papers, the page header consists of the page number only. No running head!&nbsp;</li>
	<li><a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format/font">Font</a>: Recommended fonts include&nbsp;11-point Calibri, 11-point Arial, and&nbsp;12-point Times New Roman</li>
	<li><a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format/line-spacing">Line Spacing</a>: Double-space all parts of the paper. (There are four main <a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format/line-spacing">exceptions</a>, including adding additional line spaces on the <a href="https://nwtc.libguides.com/citations/apa7#s-lg-box-22995045">Title Page</a>.)</li>
	<li><a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format/title-page">Title Page</a>:&nbsp;Should include&nbsp;the&nbsp;title, author names, author affiliation, course number and name, instructor name, assignment due date, and page number.&nbsp; <a href="https://apastyle.apa.org/instructional-aids/student-title-page-guide.pdf">Title Page Quick Guide</a></li>
	<li>Abstract Page: According to APA Style guidelines, "<a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format/order-pages">Student papers generally do not include an abstract unless requested</a>" (APA, 2022). If your instructor does require an abstract, see&nbsp;<a href="https://nwtc.libguides.com/citations/APA7#s-lg-box-28990452">Formatting the Optional Abstract Page</a> for guidelines.</li>
	<li><a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format/sample-papers">Sample Papers</a></li>
</ul>

		   </div>
			<div id="s-lg-content-63821880" class="  clearfix">
				
		   </div>
        <div id="s-lg-content-69054830" class="s-lg-widget "><iframe width="560" height="315" src="https://www.youtube.com/embed/4usdRatofC8?enablejsapi=1&amp;origin=https%3A%2F%2Fnwtc.libguides.com" title="NWTC APA Paper Format in 2 Minutes" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" data-gtm-yt-inspected-2060666_59="true" id="51719644" data-gtm-yt-inspected-8="true"></iframe>
        </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-34472703" class="s-lg-box-wrapper-34472703">
					<div id="s-lg-box-29302529-container" class="s-lib-box-container">
						<div id="s-lg-box-29302529" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Student Paper Setup Guide
                                </h2>
							<div id="s-lg-box-collapse-29302529">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-68270270" class="  clearfix">
				<p>The official APA Style &amp; Grammar Guidelines includes a <a href="https://apastyle.apa.org/instructional-aids/student-paper-setup-guide.pdf">Student Paper Setup Guide</a>: "Annotated diagrams illustrate how to set up the major sections of a student paper: the title page or cover page, the text, tables and figures, and the reference list."</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-34473041" class="s-lg-box-wrapper-34473041">
					<div id="s-lg-box-29302822-container" class="s-lib-box-container">
						<div id="s-lg-box-29302822" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Sample APA Papers
                                </h2>
							<div id="s-lg-box-collapse-29302822">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									<div class="">
                        <ul id="s-lg-link-list-70736495" class="s-lg-link-list s-lg-link-list-2"><li class="">
                        
			<div id="s-lg-content-68270793" class=""><span><a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format/sample-papers" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68270793'});">Sample Papers</a> 
				</span><div id="s-lg-link-desc-68270793" class="s-lg-link-desc">From the official <a href="https://apastyle.apa.org/style-grammar-guidelines/">APA Style &amp; Grammar Guidelines</a></div>
			</div></li>
                        <li class="">
                        
			<div id="s-lg-content-68270857" class=""><span><a href="https://owl.excelsior.edu/citation-and-documentation/apa-style/apa-sample-papers/" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68270857'});">APA Sample Papers (Excelsior University)</a> 
				</span><div id="s-lg-link-desc-68270857" class="s-lg-link-desc">From the Excelsior University Online Writing Lab's <a href="https://owl.excelsior.edu/citation-and-documentation/apa-style/">APA Style Guide</a></div>
			</div></li>
                        
                            </ul>
                            </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26984816" class="s-lg-box-wrapper-26984816">
					<div id="s-lg-box-22995045-container" class="s-lib-box-container">
						<div id="s-lg-box-22995045" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Formatting the Title Page 
                                </h2>
							<div id="s-lg-box-collapse-22995045">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-52188190" class="  clearfix">
				<p><span style="color:#000000;">For detailed explanation and sample title pages, see the official&nbsp;&nbsp;APA Style Guidelines&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines/paper-format/title-page">Title Page Setup&nbsp;section</a> and <a href="https://apastyle.apa.org/instructional-aids/student-title-page-guide.pdf">Student Title Page Guide</a>&nbsp;(PDF).</span></p>

<p>The student title page is the first page of your paper and includes:</p>

<ul>
	<li>paper title
	<ul>
		<li>centered and <strong>bolded&nbsp;</strong></li>
		<li>3-4 lines down from top of title page</li>
	</ul>
	</li>
	<li>author name
	<ul>
		<li>Add one double-spaced blank line between the paper title and author name.</li>
	</ul>
	</li>
	<li>author affiliation (Northeast Wisconsin Technical College)</li>
	<li>course number and name</li>
	<li>instructor name</li>
	<li>assignment due date</li>
</ul>

		   </div><div class="">
                        <ul id="s-lg-link-list-54924721" class="s-lg-link-list s-lg-link-list-4"><li class=" pad-bottom-sm">
                        
			<div id="s-lg-content-52193120" class="">
				<span>
					<a href="https://nwtc.libguides.com/ld.php?content_id=52193120" rel="nofollow" target="_blank" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '5',_st_content_id: '52193120'});"><i class="s-lg-file-icon fa fa-file-pdf-o" style="font-size:1.5em; margin-right:5px;"></i>Sample APA Title Page for NWTC Students
					</a>
				</span><div id="s-lg-file-desc-52193120" class="s-lg-file-desc"></div>
			</div></li>
                        
                            </ul>
                            </div>
        <div id="s-lg-content-69076458" class="s-lg-widget "><iframe width="560" height="315" src="https://www.youtube.com/embed/JYBXhL_xMoo?enablejsapi=1&amp;origin=https%3A%2F%2Fnwtc.libguides.com" title="APA Title Page Format in 2 Minutes" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" data-gtm-yt-inspected-2060666_59="true" id="268269432" data-gtm-yt-inspected-8="true"></iframe>
        </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-34090983" class="s-lg-box-wrapper-34090983">
					<div id="s-lg-box-28990452-container" class="s-lib-box-container">
						<div id="s-lg-box-28990452" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Formatting the (Optional) Abstract Page
                                </h2>
							<div id="s-lg-box-collapse-28990452">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-67465897" class="  clearfix">
				<p>According to the official APA Style&nbsp;<a href="https://apastyle.apa.org/instructional-aids/abstract-keywords-guide.pdf">Abstract and Keywords Guide</a>, "The abstract needs to provide a brief but comprehensive summary of the contents of your paper. It provides an overview of the paper and helps readers decide whether to read the full text" (APA, 2020).</p>

<p>Abstract basics:</p>

<ul>
	<li>The abstract is the second page of your paper (after the Title page).</li>
	<li>At the top of the page, center and bold Abstract.</li>
	<li>The first line of the abstract is NOT indented.</li>
	<li>APA recommends the abstract be no more than 250 words.</li>
	<li>One line below the Abstract comes the Keywords. “<a href="https://apastyle.apa.org/instructional-aids/abstract-keywords-guide.pdf" target="_blank" title="https://apastyle.apa.org/instructional-aids/abstract-keywords-guide.pdf">Keywords need to be descriptive and capture the most important aspects of your paper</a>.” Indent like a regular paragraph and type <em>Keywords</em>: (capitalized and italicized). Then in lowercase, type 3-5 words or phrases.</li>
</ul>

<p>See slide three of the Excelsior College Online Writing Lab's <a href="https://owl.excelsior.edu/citation-and-documentation/apa-style/apa-formatting-guide/" target="_blank" title="https://owl.excelsior.edu/citation-and-documentation/apa-style/apa-formatting-guide/">APA Formatting Guide</a> for a&nbsp;sample student paper Abstract page.</p>

		   </div><div class=""><ul id="s-lg-link-list-69933680" class="s-lg-link-list s-lg-link-list-4"><li class=" pad-bottom-sm"><div id="s-lg-content-67468239" class=""><span><a href="https://nwtc.libguides.com/ld.php?content_id=67468239" rel="nofollow" target="_blank" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '5',_st_content_id: '67468239',_st_inc_return: this});"><i class="s-lg-file-icon fa fa-file-pdf-o" style="font-size:1.5em; margin-right:5px;"></i>Sample APA Abstract Page for NWTC Students</a></span></div></li></ul></div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-34445766" class="s-lg-box-wrapper-34445766">
					<div id="s-lg-box-29280349-container" class="s-lib-box-container">
						<div id="s-lg-box-29280349" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Formatting the References Page
                                </h2>
							<div id="s-lg-box-collapse-29280349">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-68217476" class="  clearfix">
				<p>This is a separate page at the end of your paper. Each citation in the text must be listed on the References&nbsp;page; each listing on the References page must appear in the text.</p>

<ul>
	<li>Center and bold the word References at the top of the page</li>
	<li>All text is double-spaced, just like the rest of the paper.</li>
	<li>List the sources&nbsp;alphabetically by authors' last names.&nbsp;If no author is listed, start with the title of the article, book or web resource.</li>
	<li>Indent the second and subsequent lines of citations by 0.5 inch to create a hanging indent.
	<ul>
		<li>To do this, highlight the citation and type CTRL-T</li>
	</ul>
	</li>
</ul>

<p style="margin-left: 120px;">OR</p>

<ul style="margin-left: 40px;">
	<li>
	<p>Go to the Paragraph ribbon in Word. Click the arrow in the bottom right hand corner. This opens a box: under “special”,&nbsp;click on “hanging”.&nbsp;&nbsp;</p>
	</li>
	<li>
	<p><img alt="Paragraph ribbon" loading="lazy" src="//s3.amazonaws.com/libapps/accounts/8498/images/wordparagraph.jpg">&nbsp; &nbsp;<img alt="Hanging Indent" loading="lazy" src="//s3.amazonaws.com/libapps/accounts/8498/images/wordhangingindent.jpg"></p>
	</li>
</ul>

		   </div><div class=""><ul id="s-lg-link-list-70679567" class="s-lg-link-list s-lg-link-list-2"><li class=""><div id="s-lg-content-68217338" class=""><span><a href="https://apastyle.apa.org/instructional-aids/creating-reference-list.pdf" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '2',_st_content_id: '68217338',_st_inc_return: this});">Creating an APA Style Reference List</a></span><div id="s-lg-link-desc-68217338" class="s-lg-link-desc">2-page guide from the official <a href="https://apastyle.apa.org/style-grammar-guidelines">APA Style &amp; Grammar Guidelines</a></div></div></li></ul></div>
        <div id="s-lg-content-69194190" class="s-lg-widget "><iframe width="560" height="315" src="https://www.youtube.com/embed/OB2jA5aFnQ0?enablejsapi=1&amp;origin=https%3A%2F%2Fnwtc.libguides.com" title="NWTC APA References Page Format in 2 Minutes" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" data-gtm-yt-inspected-2060666_59="true" id="738419289" data-gtm-yt-inspected-8="true"></iframe>
        </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-32365594" class="s-lg-box-wrapper-32365594">
					<div id="s-lg-box-27521460-container" class="s-lib-box-container">
						<div id="s-lg-box-27521460" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Formatting a PowerPoint Presentation in APA Style
                                </h2>
							<div id="s-lg-box-collapse-27521460">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-63821974" class="  clearfix">
				<p>The&nbsp;<em>APA Publication Manual</em>&nbsp;does not provide specific instructions on how to format a PowerPoint presentation; however, many college libraries recommend the following:</p>

<ul>
	<li>Include the same information on your title slide that you would have on a title page.&nbsp;</li>
	<li>Include&nbsp;<a href="https://nwtc.libguides.com/citations/APA7#s-lg-box-22759078">in-text citations</a>&nbsp;for any quote, paraphrase, image, graph, table, data, audio or video file that you use within your presentation. Please note that photographs are considered figures in APA style. See the&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines/tables-figures/figures">Figure Setup section</a>&nbsp;of the&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines">APA Style &amp; Grammar Guidelines</a>&nbsp;for more information about this.</li>
	<li>The last slide will be your References page.&nbsp;</li>
	<li>“No citation, permission, or copyright attribution is necessary for clip art from programs like Microsoft Word or PowerPoint” (American Psychological Association [APA], 2020, p. 346).</li>
	<li>Do not reproduce images without permission from the creator or owner of the image. See section 12.15 of the APA manual for more information about this.</li>
</ul>

<p><a href="https://goodwin.libguides.com/c.php?g=29109&amp;p=7298502" target="_blank">How to format a PowerPoint presentation in APA Style (Goodwin College Library)</a></p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-34155948" class="s-lg-box-wrapper-34155948">
					<div id="s-lg-box-29043729-container" class="s-lib-box-container">
						<div id="s-lg-box-29043729" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Formatting an Annotated Bibliography
                                </h2>
							<div id="s-lg-box-collapse-29043729">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-67609681" class="  clearfix">
				<p>According to section 9.51 of the APA Manual,&nbsp;</p>

<ul>
	<li>"Most APA Style guidelines are applicable to annotated bibliographies" (p. 307). So, follow the <a href="https://nwtc.libguides.com/citations/apa7#s-lg-box-24847788">Paper Format guidelines</a> for&nbsp;the margins, headers, font,&nbsp;line spacing, and title page.&nbsp;</li>
	<li>"Instructors generally set all other requirements for annotated bibliographies (e.g. number of references to include, length and focus of each annotation)" (p. 307).</li>
</ul>

<p>Some basic rules:</p>

<ul>
	<li>Put the references in alphabetical order, just like you would on&nbsp;a References page.</li>
	<li>Each annotation is a new paragraph below the reference entry.&nbsp; Indent the entire annotation, just like you would a block quote (<a href="https://nwtc.libguides.com/citations/apa7#s-lg-box-22759078">using a quote that is more than 40 words)</a>.</li>
	<li>If an annotation is more than one paragraph, indent the first line of the second and any other additional paragraphs, an additional .5 inch.</li>
</ul>

<p>The Excelsior College Online Writing Lab has a <a href="https://owl.excelsior.edu/wp-content/uploads/2018/05/AnnotatedBibliographyAPA.pdf">sample APA annotated bibliography</a>.&nbsp;</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26705939" class="s-lg-box-wrapper-26705939">
					<div id="s-lg-box-22759078-container" class="s-lib-box-container">
						<div id="s-lg-box-22759078" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Citations in the Paper's Body
                                </h2>
							<div id="s-lg-box-collapse-22759078">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
        <div id="s-lg-content-70976984" class="s-lg-widget "><iframe width="560" height="315" src="https://www.youtube.com/embed/OpSPiES57s4?enablejsapi=1&amp;origin=https%3A%2F%2Fnwtc.libguides.com" title="APA in Text Citation Format in 3 Minutes" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" data-gtm-yt-inspected-2060666_59="true" id="432428907" data-gtm-yt-inspected-8="true"></iframe>
        </div>
			<div id="s-lg-content-51556603" class="  clearfix">
				<p>When you&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines/citations/quotations">quote directly</a>&nbsp;or <a href="https://apastyle.apa.org/style-grammar-guidelines/citations/paraphrasing">paraphrase</a>&nbsp;from a source (book, article, or webpage) in your paper, you need to insert an <a href="https://apastyle.apa.org/style-grammar-guidelines/citations">in-text citation</a>.&nbsp;</p>

<p>Check out the new<a href="https://apastyle.apa.org/instructional-aids/in-text-citation-checklist.pdf"> APA In-Text Citation Checklist</a>!</p>

<p>You have two format options: <a href="https://apastyle.apa.org/style-grammar-guidelines/citations/basic-principles/parenthetical-versus-narrative">parenthetical and narrative</a></p>

<h4>Parenthetical In-Text Citation</h4>

<p>This citation typically consists of the author’s last name(s), year of publication, and page number in parentheses at the end of the sentence. The period goes after the closed parenthesis.</p>

<p style="margin-left: 40px;">“This is a direct citation”&nbsp;(Chapman, 2019, p. 126).</p>

<p>When paraphrasing the idea in your&nbsp;own words, do not use quotation marks and do not include a page number&nbsp;(Jackson, 1999).</p>

<h4>Narrative In-Text Citation</h4>

<p>Another option is to use the author’s name in the sentence, followed directly by the year in parentheses, with the page numbers in parentheses at the end of the sentence.</p>

<p style="margin-left: 40px;">According to Chapman (2019), "This is a direct citation" (p. 216).</p>

<p style="margin-left: 40px;">Jackson (1999) explains that when paraphrasing the idea in your&nbsp;own words, do not use quotation marks and do not include a page number.<span style="font-size:14px;"><span style="font-family:arial,helvetica,sans-serif;"><span style="box-sizing: border-box; color: rgb(0, 0, 0);"></span></span></span><strong><strong><strong><span style="font-size:14px;"><span style="font-family:arial,helvetica,sans-serif;"></span></span></strong></strong></strong></p>

<p>Additional Resources from the <a href="https://owl.excelsior.edu/citation-and-documentation/apa-style/">Excelsior College Online Writing Lab</a>:</p>

<ul>
	<li><a href="https://owl.excelsior.edu/citation-and-documentation/apa-style/apa-in-text-citations/">APA Citations in the Body of Your Paper</a>:&nbsp;multiple examples</li>
	<li><a href="https://owl.excelsior.edu/citation-and-documentation/apa-style/apa-side-by-side/">APA Side by Side</a>: Examples that "provide reference information and corresponding in-text citation information for common source types and situations you are likely to encounter while working with your sources".&nbsp;</li>
</ul>

<h4><strong><strong><strong><span style="font-size:14px;"><span style="font-family:arial,helvetica,sans-serif;"><span style="color:#B22222;"></span><span style="background-color: rgb(255, 255, 255);"></span></span></span></strong></strong></strong></h4>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-36505263" class="s-lg-box-wrapper-36505263">
					<div id="s-lg-box-31040955-container" class="s-lib-box-container">
						<div id="s-lg-box-31040955" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Authors' Names in APA In-Text Citations
                                </h2>
							<div id="s-lg-box-collapse-31040955">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-72572469" class="  clearfix">
				<div class="row">
<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title">One Author</h3>
</div>

<div class="panel-body">
<p><strong>Narrative&nbsp;</strong></p>

<p>Chapman (2023)</p>

<p><strong>Parenthetical</strong></p>

<p>(Chapman, 2023).</p>
</div>
</div>
</div>

<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title">Two Authors</h3>
</div>

<div class="panel-body">
<p><strong>Narrative&nbsp;</strong></p>

<p>Schutz and Castleberg (2023)&nbsp;</p>

<p><strong>Parenthetical</strong></p>

<p>(Schutz &amp; Castleberg, 2023).</p>
</div>
</div>
</div>

<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title">Three or More Authors</h3>
</div>

<div class="panel-body">
<p><strong>Narrative&nbsp;</strong></p>

<p>Ornelas et al. (2023)&nbsp;</p>

<p><strong>Parenthetical</strong></p>

<p>(Ornelas et al., 2023).</p>
</div>
</div>
</div>

<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title">Group Author with Well-Known Abbreviation</h3>
</div>

<div class="panel-body">
<p><strong>Narrative&nbsp;</strong></p>

<p>First citation in text:</p>

<p>American Psychological Association (APA, 2023)</p>

<p>Subsequent citations in text:</p>

<p>APA (2023)</p>

<p><strong>Parenthetical</strong></p>

<p>First citation in text:</p>

<p>(American Psychological Association [APA], 2023).</p>

<p>Subsequent citations in text:</p>

<p>(APA, 2023).</p>
</div>
</div>
</div>

<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title">Group Author without Well-Known Abbreviation</h3>
</div>

<div class="panel-body">
<p><strong>Narrative&nbsp;</strong></p>

<p>University of Minnesota (2023)</p>

<p><strong>Parenthetical</strong></p>

<p>(University of Minnesota, 2023).</p>
</div>
</div>
</div>

<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title">No Author</h3>
</div>

<div class="panel-body">
<p>Include the title and year of publication. If the title is long (more than 3 words), shorten it.</p>

<p>If the title of the work is not italicized in the reference (article or webpage),&nbsp;put quotation marks around the title.</p>

<p>If the title of the work is italicized in the reference (book, entire website),&nbsp;italicize the title.</p>

<p>For example, if you had an article with the title Practical oral care for people with intellectual disability,&nbsp;the parenthetical citation would look like ("Practical oral care," 2014).&nbsp;</p>
</div>
</div>
</div>
</div>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-36505291" class="s-lg-box-wrapper-36505291">
					<div id="s-lg-box-31041774-container" class="s-lib-box-container">
						<div id="s-lg-box-31041774" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Direct Quoting and Paraphrasing
                                </h2>
							<div id="s-lg-box-collapse-31041774">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-72574434" class="  clearfix">
				<h4><span style="color: rgb(0, 0, 0);"><a href="https://apastyle.apa.org/style-grammar-guidelines/citations/quotations">Direct Quoting</a> -&nbsp;When you are using someone else's&nbsp;exact words.</span></h4>

<ol>
	<li>If you are using a quote that is less than 40 words, enclose the quote in quotation marks and add the&nbsp;<a href="https://nwtc.libguides.com/citations/APA7#s-lg-box-22759134">author’s name</a>&nbsp;(unless it is already in the sentence), year of publication and page numbers (if there are any)&nbsp;in parentheses.&nbsp;&nbsp;Place this reference where a pause would occur or at the end of the sentence.&nbsp;&nbsp;Punctuation marks should be placed after the parenthetical citation. You also will need to add each work from which you cite to your Works Cited page. Here are two examples:

	<ul>
		<li>
		<p>The article goes on to say that “People don't do derby just for exercise but usually because it becomes a part of who they are” (Fagundes, &nbsp;2012, p.1098).</p>
		</li>
		<li>
		<p>Fagundes&nbsp;(2012) believes that roller derby gives participants "a chance to feel like a superstar" (p. 1098).</p>
		</li>
	</ul>
	</li>
	<li>If you are using a quote that is more than 40 words,&nbsp;do not use quotation marks. Instead, put the quote on a new line and indent the whole block approximately 1/2 inch from the left margin. Keep the quote double-spaced. Remember to add a parenthetical citation and put the work on your References&nbsp;page.&nbsp; The&nbsp;parenthetical citation comes after the final punctuation mark. This is called a <a href="https://apastyle.apa.org/style-grammar-guidelines/citations/quotations"><strong>Block Quotation</strong></a>.</li>
</ol>

<p>He asserts the following:</p>

<p style="margin-left: 40px;">More importantly, though, the notion of competing under derby names was a perfect fit with the recent reimagination of the sport as a punk-rock spectacle that allowed, and encouraged, participants to develop outrageous public personas. The story of derby-name emergence probably has more to do with coincidence and path dependence than with conscious design. Derby pioneer Ivanna S. Pankin’s classic derby name pre-dated her founding of Arizona Roller Derby in 2003. Rather, it was a handle and email address she used as a musician in Phoenix’s punk rock scene. When she publicized her nascent league using the alias Ivanna S. Pankin, and the entire Austin scene was already using skate names, the leagues that popped up in their wake followed suit,33 and the practice of using colorful nicknames has been used by virtually all derby leagues and skaters since.&nbsp;(Fagundes, 2012, pp.1093-1094)</p>

<p style="color: rgb(85, 85, 85); line-height: 21px;"><span style="font-size: 14px;"><span style="font-family: arial, helvetica, sans-serif;"><span style="color: rgb(0, 0, 0);"></span></span></span></p>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);"><a href="https://apastyle.apa.org/style-grammar-guidelines/citations/paraphrasing">Paraphrasing</a>&nbsp;-&nbsp;Restating others' ideas in your own words.</h4>

<ul>
	<li>
	<p>If you mention the author’s name in the paragraph, then just put the date in parentheses directly after the author's name.</p>

	<center>Fagundes (2012) believes it is hard to pin down when the practice of skating under a pseudonym began.</center>
	</li>
</ul>

<p>&nbsp;</p>

<ul>
	<li>
	<p>If you do not mention the author’s name, then include the author’s name in parentheses before the date.</p>

	<center>It is hard to pin down when the practice of skating under a pseudonym began (Fagundes, 2012).</center>
	</li>
</ul>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-33704747" class="s-lg-box-wrapper-33704747">
					<div id="s-lg-box-28661952-container" class="s-lib-box-container">
						<div id="s-lg-box-28661952" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Capitalization
                                </h2>
							<div id="s-lg-box-collapse-28661952">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-66596936" class="  clearfix">
				<p>According to the APA Style Guide&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines/capitalization">section on Capitalization</a>, "APA Style is a 'down'&nbsp;style, meaning that words are lowercase unless there is specific guidance to capitalize them."&nbsp;&nbsp;</p>

<p>Here are some basic rules for capitalizing within APA papers:</p>

<p><a href="https://apastyle.apa.org/style-grammar-guidelines/capitalization/diseases-disorders-therapies">Diseases, Disorders, Therapies, and More</a>&nbsp;</p>

<ul>
	<li>In general, do not capitalize the names of diseases,&nbsp;treatments, theories, concepts, etc.</li>
	<li>Do capitalize&nbsp;personal names that appear within these kinds of terms.</li>
	<li>Examples: lung cancer, art therapy, Alzheimer's disease, Adlerian therapy</li>
</ul>

<p><a href="https://apastyle.apa.org/style-grammar-guidelines/capitalization/proper-nouns">Proper Nouns, Trade Names, and Generic Drug Names</a></p>

<ul>
	<li>Capitalize proper nouns, names of racial &amp;&nbsp;ethnic groups, and trade &amp; brand names.</li>
	<li>Do not capitalize generic drug names.</li>
	<li>Example: Prozac (trade name) vs&nbsp;fluoxetine (generic name)</li>
</ul>

<p>When in doubt,&nbsp;<a href="https://nwtc.libanswers.com/">ask a librarian</a>&nbsp;or consult the&nbsp;<a href="https://dictionary.apa.org/">APA Dictionary of Psychology</a>.</p>
		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26714944" class="s-lg-box-wrapper-26714944">
					<div id="s-lg-box-22766629-container" class="s-lib-box-container">
						<div id="s-lg-box-22766629" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">No page numbers? 
                                </h2>
							<div id="s-lg-box-collapse-22766629">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-51578622" class="  clearfix">
				<p><span style="color:#000000;">According to&nbsp;the official APA Style &amp; Grammar Guidelines&nbsp;sections on <a href="https://apastyle.apa.org/style-grammar-guidelines/citations/basic-principles/parts-source">Citing Specific Parts of a Source</a>&nbsp;and&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines/citations/quotations/no-page-numbers">Direct&nbsp;Quotation of Materials without Page Numbers</a>, you have several options&nbsp;when quoting sources that do not have page numbers (webpages, eBooks, etc.). Use the option that will best help your reader find the quotation in the source.</span></p>

<h3><span style="font-size:16px;">Provide a paragraph number; you will probably have to count them manually.</span></h3>

<p><span style="color:#000000;">According to the IceBridge&nbsp;Project leader, "&nbsp;in addition to the airborne and satellite measurements, scientists will be out on the ice taking height and density measurements as well"&nbsp;(Gray, 2019, para. 6). </span><a href="https://blogs.nasa.gov/earthexpeditions/2019/10/24/icebridge-takes-flight-from-down-under/">Source</a></p>

<h3><span style="font-size:16px;">Provide a heading or section name.</span></h3>

<p><span style="color:#000000;">Medical consensus is that the flu is spread&nbsp;"</span>mainly by tiny droplets made when people with flu cough, sneeze or talk"<span style="color:#000000;">&nbsp;(Centers for Disease Control and Prevention, 2019, How Flu Spreads section).&nbsp;<a href="https://www.cdc.gov/flu/about/keyfacts.htm" style="background-color: rgb(255, 255, 255); color: rgb(35, 82, 124); text-decoration-line: underline; outline: 0px;">Source</a></span></p>

<h3><span style="font-size:16px;">Provide an abbreviated heading or section name in quotation marks if the name is too long or unwieldy to cite in full.</span></h3>

<p><span style="color:#000000;"><span style="color: rgb(0, 0, 0);">Research has shown that an average of 8% of the U.S. population experiences flu symptoms each flu season,"</span></span>&nbsp;with a range of between 3% and 11%, depending on the season"<span style="color:#000000;"><span style="color: rgb(0, 0, 0);">&nbsp;</span>(Centers for Disease Control and Prevention, 2019, "How Many People" section). <a href="https://www.cdc.gov/flu/about/keyfacts.htm">Source</a></span></p>

<h3><span style="font-size:16px;">Provide a heading or section name, plus a paragraph number within that section.</span></h3>

<p><span style="color:#000000;">Thompson's research is focused on understanding "factors that support speech-in-noise ability in early childhood"&nbsp;(DeAngelis, 2018, Auditory neurodevelopment&nbsp;section, para. 4).&nbsp;<a href="https://www.apa.org/monitor/2018/07-08/auditory-system.html">Source</a></span></p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-36515159" class="s-lg-box-wrapper-36515159">
					<div id="s-lg-box-31045963-container" class="s-lib-box-container">
						<div id="s-lg-box-31045963" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Authors' Names on the References Page
                                </h2>
							<div id="s-lg-box-collapse-31045963">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-72584612" class="  clearfix">
				<div class="row">
<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title"><a href="https://apastyle.apa.org/style-grammar-guidelines/references/elements-list-entry#author">One Author</a></h3>
</div>

<div class="panel-body">
<p>Invert all individual authors’ names, providing the last name&nbsp;first, followed by a comma and the author’s initials.</p>

<p>Ornelas, J. N.&nbsp;</p>

<p>Rockwell-Kincanon, K.</p>
</div>
</div>
</div>

<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title">Two to 20 Authors</h3>
</div>

<div class="panel-body">
<p>List by their last names and initials, separated by a comma. Put an &amp; between the final two names.</p>

<p>Knowles-Carter, B.,&nbsp;Carter, B.I., &amp;&nbsp;Carter, S.</p>
</div>
</div>
</div>

<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title"><a href="https://apastyle.apa.org/blog/more-than-20-authors">21 or More Authors</a></h3>
</div>

<div class="panel-body">
<p>Include the first 19 authors, insert an ellipsis ... (but no &amp;), and then add the final author’s name:</p>

<p>Author, A. A., Author, B. B., Author, C. C., Author, D. D., Author, E. E., Author, F. F., Author, G. G., Author, H. H., Author, I. I., Author, J. J., Author, K. K., Author, L. L., Author, M. M., Author, N. N., Author, O. O., Author, P. P., Author, Q. Q., Author, R. R., Author, S. S., . . .&nbsp;Author, Z. Z.. (2018).&nbsp;</p>
</div>
</div>
</div>

<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title"><a href="https://apastyle.apa.org/style-grammar-guidelines/references/elements-list-entry#author">Group Author&nbsp;</a></h3>
</div>

<div class="panel-body">
<p>Spell out the full name of a group author, such as an organization or government agency.</p>

<p>American Psychological Association.</p>

<p>Centers for Disease Control and Prevention.</p>
</div>
</div>
</div>

<div class="col-sm-4">
<div class="panel panel-default">
<div class="panel-heading">
<h3 class="panel-title"><a href="https://apastyle.apa.org/style-grammar-guidelines/references/missing-information">No Author</a></h3>
</div>

<div class="panel-body">
<p>Move the title of the source to the beginning of the entry, before the publication date.</p>

<p><strong>Articles &amp; Webpages</strong></p>

<p>Generalized anxiety disorder. (2020).&nbsp;</p>

<p>In-text citation: "Generalized anxiety disorder," 2020).</p>

<p><strong>Books</strong></p>

<p>Interpersonal skills. (2020).</p>

<p>In-text citation: (<em>Interpersonal Skills</em>, 2020).</p>
</div>
</div>
</div>
</div>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26719144" class="s-lg-box-wrapper-26719144">
					<div id="s-lg-box-22770224-container" class="s-lib-box-container">
						<div id="s-lg-box-22770224" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Journal Articles
                                </h2>
							<div id="s-lg-box-collapse-22770224">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-51589303" class="  clearfix">
				<ul>
	<li><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/journal-article-references">APA </a>recommends providing a <a href="https://apastyle.apa.org/style-grammar-guidelines/references/dois-urls">Digital Object Identifier (DOI)</a>, when it is available. DOIs provide stable, long-lasting links for online articles.</li>
	<li>If there is no DOI and the journal article&nbsp;is from a library research database, end the reference after the page number.&nbsp;The reference in this case will look the&nbsp;the same as for a print journal article.</li>
	<li>If the journal article does not have a DOI and is not from a library database, but does have a URL that will resolve, include the URL of the article at the end of the reference.</li>
</ul>

<h4>Basic Scholarly Journal Article Format</h4>

<p>Author, A. A., Author, B. B., &amp; Author, C. C. (Year). Title of article: Subtitle words. <em>Title of Periodical, volume number</em>(issue number), pages. https://doi.org/xx.xxx/yyyyy</p>

<p><a href="https://apastyle.apa.org/instructional-aids/journal-article-reference-checklist.pdf">Journal Article Reference Checklist</a>&nbsp;(from the official <a href="https://apastyle.apa.org/style-grammar-guidelines">APA Style and Grammar Guidelines</a>)</p>

<h4>Example of Scholarly Article with DOI</h4>

<p>Nguyen, T. T., Gildengorin, G., &amp; Truong, A. (2007). Factors influencing physicians' screening behavior for liver cancer among high-risk patients. <em>Journal of General Internal Medicine, 22</em>(4), 523-526. https://doi.org/10.1007/s11606-007-0128-1</p>

<h4>Example of Scholarly Article from Library Database with No DOI</h4>

<p>Ryan,E., &amp; Redding, R. (2004). A review of mood disorders among juvenile offenders. <em>Psychiatric Services, 55</em>(12), 1397-1407.</p>

<h4>Example of Scholarly Article from Website with No DOI</h4>

<p>Humphreys, B. L. (2002). Adjusting to progress: Interactions between the National Library of Medicine and health sciences librarians, 1961-200. <em>Journal of the Medical Library Association</em>, <em>90</em>(1), 4-20. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC64753/</p>

		   </div>
        <div id="s-lg-content-73407907" class="s-lg-widget "><iframe width="560" height="315" src="https://www.youtube.com/embed/IHUZ0InywrQ?si=Y-Tr2n97-SvMPIYz&amp;enablejsapi=1&amp;origin=https%3A%2F%2Fnwtc.libguides.com" title="Citing a Journal Article in APA Format" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen="" data-gtm-yt-inspected-2060666_59="true" id="684790669" data-gtm-yt-inspected-8="true"></iframe>
        </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26719179" class="s-lg-box-wrapper-26719179">
					<div id="s-lg-box-22770252-container" class="s-lib-box-container">
						<div id="s-lg-box-22770252" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Magazine Articles
                                </h2>
							<div id="s-lg-box-collapse-22770252">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-51589383" class="  clearfix">
				<ul>
	<li><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/journal-article-references">APA </a>recommends providing a <a href="https://apastyle.apa.org/style-grammar-guidelines/references/dois-urls">Digital Object Identifier (DOI)</a>, when it is available. DOIs provide stable, long-lasting links for online articles.</li>
	<li>If there is no DOI and the magazine article&nbsp;is from a library research database, end the reference after the page number.&nbsp;The reference in this case will look the&nbsp;the same as for a print journal article.</li>
	<li>If the magazine article does not have a DOI and is not from a library database, but does have a URL that will resolve, include the URL of the article at the end of the reference.</li>
</ul>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">Basic Magazine Article Format</h4>

<p>Author, A. A., Author, B. B., &amp; Author, C. C. (Year, Month Day). Title of article: Subtitle words.&nbsp;<em>Title of Magazine, volume number</em>(issue number), pages. https://doi.org/xx.xxx/yyyyy</p>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">Example of Magazine Article with DOI</h4>

<p>Schaefer, N. K., &amp; Shapiro, B. (2019, September 6). New middle chapter in the story of human evolution. <em>Science, 365</em>(6457), 981-982. https://doi.org/10.1126/science.aay3550</p>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">Example of Magazine Article from Library Database with no DOI</h4>

<p>Lane, A., &amp; Brody, R. (2019). No laughing matter. <em>New Yorker, 95</em>(41), 65-67.</p>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">Example of Magazine Article from Website with URL</h4>

<p>Hall, M. (2017, March). The faces of Obamacare. <em>Texas Monthly, 45</em>(3), 116-197.&nbsp;https://www.texasmonthly.com/news-politics/the-faces-of-obamacare/</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-28830716" class="s-lg-box-wrapper-28830716">
					<div id="s-lg-box-24565494-container" class="s-lib-box-container">
						<div id="s-lg-box-24565494" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Newspaper Articles
                                </h2>
							<div id="s-lg-box-collapse-24565494">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-56295880" class="  clearfix">
				<h4><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/newspaper-article-references">Basic Newspaper Article Format</a></h4>

<ul>
	<li>If the newspaper article is from a library research database, end the reference after the page number.&nbsp;The reference in this case will look the&nbsp;the same as for a print newspaper article.</li>
	<li>If the newspaper article is from an online newspaper website, include the URL of the article at the end of the reference.&nbsp;</li>
	<li>If the article is from a news website (e.g., CNN, HuffPost)—one that does not have an associated daily or weekly newspaper—use the format for a&nbsp;<a href="https://www.apastyle.org/style-grammar-guidelines/references/examples/webpage-website-references#1">webpage on a news website</a>&nbsp;instead.</li>
</ul>

<p>Author, A. A.&nbsp;(Year, Month Day). Title of article: Subtitle words.&nbsp;<em>Title of Newspaper, volume number</em>(issue number), pages.&nbsp;</p>

<h4>Examples of Newspaper Article from Library Database&nbsp;</h4>

<p>Seitz, M. J. (2020, June 26).&nbsp; El Paso's Bishop Mark Seitz: Black lives matter. <em>National Catholic Reporter</em>, 13-16.</p>

<p>Bowles, N. (2019, July 7). Virtual pre-K closes a gap, and exposes it. <em>New York Times, 168</em>(58381)&nbsp;, 1, 15.</p>

<h4>Example of Newspaper Article from Website with URL</h4>

<p>Kowols, T. (2022, June 29). Sturgeon Bay Police issues warnings about fireworks. <em>Door County Daily News</em>. https://doorcountydailynews.com/news/641257</p>

<h4>Example of a <a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/webpage-website-references#1">Webpage on a News Website</a>&nbsp;</h4>

<p>Strickland, A. (2023, February 21). <em>Earth’s driest place shows why it may be harder than we thought to find signs of life on Mars</em>. CNN.&nbsp;https://www.cnn.com/2023/02/21/world/atacama-desert-life-mars-scn/index.html</p>

<p>&nbsp;</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-37040382" class="s-lg-box-wrapper-37040382">
					<div id="s-lg-box-31488680-container" class="s-lib-box-container">
						<div id="s-lg-box-31488680" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Blog Post
                                </h2>
							<div id="s-lg-box-collapse-31488680">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-73676540" class="  clearfix">
				<h4><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/blog-post-references#1">Basic Format for a Blog Post</a></h4>

<ul>
	<li>Blog posts follow the same format as <a href="https://nwtc.libguides.com/citations/apa7#s-lg-box-22770224">journal articles</a>.</li>
	<li>Italicize the name of the blog, the same as you would a journal title.</li>
</ul>

<p>Meinholz, G. (2023, May 12). In Jordan Love we trust. <em>Packers Talk Blog</em>. https://packerstalk.com/2023/05/12/in-jordan-love-we-trust/</p>

<p>&nbsp;</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26710141" class="s-lg-box-wrapper-26710141">
					<div id="s-lg-box-22762598-container" class="s-lib-box-container">
						<div id="s-lg-box-22762598" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Webpage: Individual Author
                                </h2>
							<div id="s-lg-box-collapse-22762598">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-51567576" class="  clearfix">
				<h4><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/webpage-website-references#5">Basic Format</a>&nbsp;for Webpage or Document on an Organization or&nbsp;Government Website: Individual Author</h4>

<p>Author, A. A.&nbsp;(Date published or updated).&nbsp;&nbsp;<em>Title of report or document: Subtitle of report. </em>Organization Name<em>.&nbsp;</em>http://someurl</p>

<p>&nbsp;</p>

<h4>Example of Webpage on an Organization Website</h4>

<p>Schaeffer, K. (2022, April 5). <em>In CDC survey, 37% of U.S. high school students report regular mental health struggles during COVID-19</em>. Pew Research Center.&nbsp;https://www.pewresearch.org/fact-tank/2022/04/25/in-cdc-survey-37-of-u-s-high-school-students-report-regular-mental-health-struggles-during-covid-19/</p>

<p>&nbsp;</p>

<h4>Example of Webpage on a Government Agency** Website</h4>

<p>**Make sure to include the names of the parent department(s)&nbsp;and&nbsp;specific agency/center/office (hierarchy).&nbsp;</p>

<p>Hoyert, D. L. (2021, March 23). <em>Maternal mortality rates in the United States, 2019</em>.&nbsp;U.S. Department of Health and Human Services, Centers for Disease Control &amp; Prevention, National Center for Health Statistics.&nbsp;https://www.cdc.gov/nchs/data/hestat/maternal-mortality-2021/maternal-mortality-2021.htm</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26710116" class="s-lg-box-wrapper-26710116">
					<div id="s-lg-box-22762581-container" class="s-lib-box-container">
						<div id="s-lg-box-22762581" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Webpage: Government Agency as Group Author
                                </h2>
							<div id="s-lg-box-collapse-22762581">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-51567464" class="  clearfix">
				<p><span style="color:#000000;"></span></p>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);"><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/webpage-website-references#3"><span style="color:#0000ff;">Basic&nbsp;Format</span></a><span style="color:#0000ff;">&nbsp;</span></h4>

<ul>
	<li><span style="color:#000000;">Spell out the full name of a group author in the reference list entry, followed by a period.</span></li>
	<li><span style="color:#000000;">While an&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines/abbreviations/group-authors">abbreviation for the group author</a>&nbsp;can be used in the text (e.g., NIMH for National Institute of Mental Health), do NOT&nbsp;include an abbreviation for a group author in a reference list entry.</span></li>
	<li><span style="color: rgb(0, 0, 0);">When numerous layers of government agencies are listed as the author of a work, use the most specific agency as the author in the reference. The names of parent agencies appear after the title&nbsp;as the publisher.</span></li>
</ul>

<p>&nbsp;</p>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);"><span style="color: rgb(0, 0, 0);">Examples of Documents or Webpages on a Government Website</span></h4>

<p>Division for Heart Disease and Stroke Prevention. (2021, March 29). <i>Educational materials for health professionals</i>. U.S. Department of Health &amp; Human Services, Centers for Disease Control, National Center for Chronic Disease Prevention and Health Promotion.&nbsp;https://www.cdc.gov/dhdsp/materials_for_professionals.htm</p>

<div><span style="color:#000000;">&nbsp;</span></div>

<p><span style="color:#000000;">National Heart, Lung, and Blood Institute. (2022, March 24). <i>Heart treatments</i>. U.S. Department of Health &amp; Human Services, National Institutes of Health. https://www.nhlbi.nih.gov/health-topics/cardiac-rehabilitation&nbsp;</span></p>

<p>&nbsp;</p>

<p>National Institute of&nbsp;Mental Health. (2020, January). <em>Coping with traumatic events</em>.&nbsp;&nbsp;<span style="color:#000000;">U.S. Department of Health &amp; Human Services, National Institutes of Health.&nbsp;https://www.nimh.nih.gov/health/topics/coping-with-traumatic-events</span></p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-34092254" class="s-lg-box-wrapper-34092254">
					<div id="s-lg-box-28991525-container" class="s-lib-box-container">
						<div id="s-lg-box-28991525" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Webpage: Organization as Group Author
                                </h2>
							<div id="s-lg-box-collapse-28991525">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-67473401" class="  clearfix">
				<h4><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/webpage-website-references#4">Basic Format</a></h4>

<p>Name of Organization. (Date published or updated). <em>Title of webpage or document: Subtitle of documen</em>t.&nbsp;http://someurl</p>

<h4>Examples</h4>

<p>American Nurses Association. (2022).&nbsp;<em>Nurses bill of rights</em>.&nbsp;https://www.nursingworld.org/practice-policy/work-environment/health-safety/bill-of-rights/</p>

<p>Wisconsin Psychological Association. (2022). <em>Mental health information</em>.&nbsp;https://wipsychology.org/Mental_Health_Information</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26719229" class="s-lg-box-wrapper-26719229">
					<div id="s-lg-box-22770292-container" class="s-lib-box-container">
						<div id="s-lg-box-22770292" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Books/E-Books and Book Chapters
                                </h2>
							<div id="s-lg-box-collapse-22770292">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-51589503" class="  clearfix">
				<ul>
	<li style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">Use the same formats for both print books and e-books. For e-books, the format (e.g. PDF, EPUB), platform (e.g. Ebook Central, OpenStax), or device (e.g., Kindle) is not included in the reference.</li>
	<li style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">If an&nbsp;e-book without a DOI has a stable URL that will resolve for all readers, include the URL of the book.&nbsp;</li>
	<li style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">Include a&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines/references/dois-urls">Digital Object Identifier (DOI)</a>, when it is available.</li>
</ul>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);"><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/book-references">Basic Book Formats</a></h4>

<p>Author, A. A., Author, B. B., &amp; Author, C. C. (Year). <em>Title of book: Subtitle words</em>. Publisher Name.</p>

<p>Author, A. A. (Year).&nbsp;Title of chapter. In A. A. Editor &amp; B. B. Editor (Eds.),&nbsp;<em>&nbsp;Title of book&nbsp;</em>(pages of chapter).&nbsp;Publisher Name.</p>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);"><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/book-references#1">Print Book/E-Book</a>&nbsp;</h4>

<p>Bonilla-Silva, E. (2017). <em>Racism without the racists: Color-blind racism and the persistence of racial inequality in America</em>.&nbsp;Rowman &amp; Littlefield.</p>

<p>Olsen, Y., &amp; Sharfstein, J. M. (2019). <em>The opioid epidemic: What everyone needs to know</em>. Oxford University Press.</p>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);"><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/edited-book-chapter-references">Chapter in an Edited Book</a>&nbsp;</h4>

<p>Kindler, L. L., &amp; Polomano, R. C. (2017). Pain. In S. L. Lewis, S. R. Dirksen, M. M. Heitkemper, &amp; L. Bucher (Eds.), <em>Medical-surgical nursing</em> (10th ed., pp. 114-139). Elsevier.</p>

<p>Scott, C.L. (2014). Historical perspectives for studying workforce diversity. In M.Y. Byrd (Ed.), <em>Diversity in the workforce: Current issues and emerging trends</em> (pp. 3-33). Routledge.</p>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);"><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/book-references#2">Whole Edited Book</a>&nbsp;</h4>

<p>Pedersen, P. B., Lonner, W. J., Draguns, J. G., Trimble, J.E., &amp; Scharrón-del Río, M. R. (Eds.). (2016). <em>Counseling across cultures</em> (7th ed.). Sage.</p>

<p>Ramos, K. S., Downey, A., &amp; Yost, O. C. (Eds.). <em>Nonhuman primate models in biomedical research: State of the science and future needs</em>. National Academies Press.&nbsp;https://nap.nationalacademies.org/read/26857/chapter/1</p>

<p>&nbsp;</p>

<p>For more examples, see the <a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/book-references">Book/Ebook References section</a> on the <a href="https://apastyle.apa.org/style-grammar-guidelines/">APA Style &amp; Grammar Guidelines</a></p>

<p>&nbsp;</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-29220600" class="s-lg-box-wrapper-29220600">
					<div id="s-lg-box-24883932-container" class="s-lib-box-container">
						<div id="s-lg-box-24883932" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Published Dissertation or Thesis
                                </h2>
							<div id="s-lg-box-collapse-24883932">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-57125032" class="  clearfix">
				<p><span style="color:#000000;">A dissertation or thesis is considered published when it is available from a database such as ProQuest Dissertations and Theses,&nbsp;an institutional repository, or an archive.</span> (<a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/published-dissertation-references">Published Dissertation or Thesis References</a>)</p>

<h4><span style="color:#000000;">Basic&nbsp;Format</span></h4>

<p><span style="color:#000000;">Author, A. A. (Date published). <em>Title of dissertation: Subtitle of dissertation&nbsp;</em>[Doctoral dissertation, Name of University]. Name of Repository or Database. url</span></p>

<p><span style="color:#000000;">Author, A. A. (Date published). <em>Title of thesis: Subtitle of thesis&nbsp;</em>[Master's thesis, Name of University]. Name of Repository or Database. url</span></p>

<h4><span style="color:#000000;">Examples</span></h4>

<p><span style="color:#000000;">Robinson, G. D. (2019). <em>Promoting persistence among LGBTQ community college students</em> [Doctoral dissertation, Illinois State University]. ISU ReD Repository. https://ir.library.illinoisstate.edu/cgi/viewcontent.cgi?article=2041&amp;context=etd</span></p>

<p>Lindmark, S. A. (2019).&nbsp;"W<em>atching their souls speak": Interpreting the new music videos of Childish Gambino, Kendrick Lamar, and Beyoncé Knowles-Carter</em> [Master's thesis, UC Irvine].&nbsp;University of California eScholarship.&nbsp;https://escholarship.org/content/qt5gw3v7bf/qt5gw3v7bf.pdf</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26719328" class="s-lg-box-wrapper-26719328">
					<div id="s-lg-box-22770371-container" class="s-lib-box-container">
						<div id="s-lg-box-22770371" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Online Videos
                                </h2>
							<div id="s-lg-box-collapse-22770371">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-51590384" class="  clearfix">
				<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);"><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/youtube-references">YouTube Video Basic Format</a></h4>

<p>Author, A.A. [Screen name]. (year, month day).&nbsp;<em>Title of video</em>&nbsp;[Video]. YouTube.&nbsp;http://xxxxx</p>

<ul>
	<li>
	<p>Use the name of the account that uploaded the video as the author.</p>
	</li>
	<li>
	<p>If the user’s real name is not available, include only the screen name, without brackets</p>
	</li>
	<li>
	<p>The capitalization [or lack thereof] in the screen name is in keeping with how it appears online.</p>
	</li>
	<li>In text, cite by the author name that appears outside of brackets, whichever one that may be.</li>
</ul>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">&nbsp;</h4>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">YouTube Video Examples</h4>

<p>Bozeman Science. (2014, March 10).&nbsp;<em>Integumentary system</em> [Video]. YouTube.&nbsp;https://youtu.be/z5VnOS9Ke3g</p>

<p>Fox, D. J. [Dr. Daniel Fox]. (2019, February 19).&nbsp;<em>Empathy paradox and borderline personality disorder</em> [Video]. YouTube.&nbsp;https://youtu.be/mGa3tQCoJ-E</p>

<p>&nbsp;</p>

<h4><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/ted-talk-references">TED Talks</a></h4>

<p style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">TED Talk from YouTube</p>

<ul>
	<li>
	<p>Follow the YouTube video basic format shown above.</p>
	</li>
	<li>
	<p>List the owner of the YouTube account as&nbsp;the author. In most cases, it will be the <a href="https://www.youtube.com/c/TED/videos">TED YouTube account</a>.</p>
	</li>
	<li>Credit YouTube as the publisher of the&nbsp;TED&nbsp;Talk and then provide the URL.</li>
</ul>

<p>TED. (2022, May 10). <em>An Olympic champion's mindset for overcoming fear&nbsp;| Allyson Felix</em>&nbsp;[Video]. YouTube.&nbsp;https://www.youtube.com/watch?v=iIne-UO7wUo</p>

<p style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">TED Talk from the TED website</p>

<ul>
	<li>
	<p>Use the name of the speaker as the author.</p>
	</li>
	<li>
	<p>Include [Video] after the title of the talk.</p>
	</li>
	<li>
	<p>Credit TED Conferences as the publisher of the TED Talk and end with the URL.</p>
	</li>
</ul>

<p>Schor, J. (2022, April). <em>The case for the 4-day work week </em>[Video]. TED Conferences.&nbsp;https://www.ted.com/talks/juliet_schor_the_case_for_a_4_day_work_week</p>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">&nbsp;</h4>

<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);">Films on Demand Video Basic Format</h4>

<p><em>Title of video: Subtitle of video</em>&nbsp;[Video]. (year). Films on Demand.&nbsp;http://xxxxx</p>

<p><span style="font-size: 18px;">Films on Demand Video Examples</span></p>

<p><span style="color:#000000;"><em>&#xFEFF;Method and madness: In search of science</em>&nbsp;[Video]. (2013). Films on Demand. https://fod.infobase.com/PortalPlaylists.aspx?wID=102581&amp;xtid=55680</span></p>

<p><span style="color:#000000;"><em>Suctioning: Nasotracheal suctioning and monitoring complications</em>&nbsp;[Video]. (2018). Films on Demand. https://fod.infobase.com/PortalPlaylists.aspx?wID=102581&amp;xtid=154477</span></p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26719925" class="s-lg-box-wrapper-26719925">
					<div id="s-lg-box-22770851-container" class="s-lib-box-container">
						<div id="s-lg-box-22770851" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">PowerPoint Slides
                                </h2>
							<div id="s-lg-box-collapse-22770851">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-51591119" class="  clearfix">
				<h4 style="font-family: Arial, Helvetica, Verdana; color: rgb(51, 51, 51);"><a href="https://apastyle.apa.org/style-grammar-guidelines/references/examples/powerpoint-references#2">PowerPoint Slides Accessed on Canvas: Basic Format</a></h4>

<ul>
	<li>If you are writing for an NWTC audience with access to the specific Canvas course, provide the name of the site and its URL (use the login page URL for sites requiring login).</li>
	<li>If the audience for which are you writing does not have access to the slides,&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines/citations/personal-communications">cite them as a personal communication</a>.</li>
</ul>

<p>Instructor last name, Initial. (year).<em> Title of PowerPoint presentation</em> [PowerPoint slides]. NWTC Canvas. https://nwtc.instructure.com/login/saml</p>

<p><span style="font-size: 18px;">PowerPoint Slides Example</span></p>

<p>Chapman, J. M. (2019). <em>Overview of citations: What's new with APA</em> [PowerPoint slides].&nbsp;NWTC Canvas. https://nwtc.instructure.com/login/saml</p>

<p><span style="color: rgb(0, 0, 0);"></span><span style="color: rgb(0, 0, 0);"></span></p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-26720058" class="s-lg-box-wrapper-26720058">
					<div id="s-lg-box-22770982-container" class="s-lib-box-container">
						<div id="s-lg-box-22770982" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Interviews
                                </h2>
							<div id="s-lg-box-collapse-22770982">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-51591551" class="  clearfix">
				<p>If you want to cite an interview, email, chat, text message or other <a href="https://apastyle.apa.org/style-grammar-guidelines/citations/personal-communications">personal communication</a>, you only need to do so as a parenthetical citation in the body of your paper; you do NOT need to include it in your References.</p>

<p>Use this format for the parenthetical citation:</p>

<p>(A. Lastname, personal communication, date of communication).</p>

<p>Example:</p>

<p>(P. Malone, personal communication, December 3, 2020).</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-34098839" class="s-lg-box-wrapper-34098839">
					<div id="s-lg-box-28996741-container" class="s-lib-box-container">
						<div id="s-lg-box-28996741" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Legal Sources (Court Cases, Legislation, Statutes)
                                </h2>
							<div id="s-lg-box-collapse-28996741">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-67482853" class="  clearfix">
				<p>The&nbsp;<a href="https://apastyle.apa.org/style-grammar-guidelines">official APA Style &amp; Grammar Guidelines</a>&nbsp;do not include reference examples for legal references, such as court cases.&nbsp;</p>

<p>Chapter 11 Legal References of the&nbsp;<a href="https://wispals3.iii.com/iii/encore_nwtc/record/C__Rb3853225__S9781433832154__Orightresult__U__X3?lang=eng&amp;suite=nwtc" onclick="return springSpace.springTrack.trackLink({link: this,_st_type_id: '3',_st_content_id: '52542604'});">Publication Manual of the American Psychological Association, 7th edition</a>&nbsp;begins with this statement: "In APA Style, most legal materials are cited in the standard legal citation&nbsp;style used for legal references across all disciplines" (p. 355).</p>

<p>Chapter 11 does contain some&nbsp;examples of common legal references (see below), but recommends consulting <em>The Bluebook: A Uniform System of Citation</em>. There is a copy of the 21st edition (2020) at the Green Bay Library Desk, as well as copies of the 20th edition (2015) in the Reference Collections in Green Bay, Marinette, and Sturgeon Bay.</p>

<h4>Federal Statutes (Laws and Acts)</h4>

<p>The template for federal statutes is:</p>

<p>Reference list: Name of Act, Title Source&nbsp;§ Section Number (Year). url</p>

<p>Americans with Disabilities Act of 1990, 42 U.S.C.&nbsp;§ 12101 (1990). https://www.ada.gov/pubs/adastatute08.htm</p>

<p>Parenthetical citation: (Name of Act, Year).</p>

<p>Narrative citation: Name of Act (Year)</p>

<h4>Supreme Court Case, With Page Number (cases published through 2012 term)</h4>

<p>Name of Party v. Name of Party, volume number U.S. page number (year of decision). url</p>

<p>Brown v. Board of Education, 347&nbsp;U.S. 483&nbsp;(1954). <a href="https://www.loc.gov/item/usrep410113/">https://www.loc.gov/item/usrep347483/</a><span style="font-size:12.0pt"><span style="font-family:&quot;Times New Roman&quot;,serif"> </span></span></p>

<p>In-text citation: (<em>Brown v. Board of Education,</em> 1954)</p>

<h4>Supreme Court Case, Without Page Number (cases published after 2012 term)</h4>

<p>Name of Party v. Name of Party, volume number U.S.&nbsp;___&nbsp;(year of decision). url</p>

<p>Gallardo v. Marstiller, 596 U.S. ___ (2022).&nbsp; https://www.oyez.org/cases/2021/20-1263</p>

<p>In-text citation: (<em>Gallardo&nbsp;v. Marstiller,</em>&nbsp;2022)</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-32715619" class="s-lg-box-wrapper-32715619">
					<div id="s-lg-box-27819773-container" class="s-lib-box-container">
						<div id="s-lg-box-27819773" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">Oral Teachings of Indigenous Elders and Knowledge Keepers
                                </h2>
							<div id="s-lg-box-collapse-27819773">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-64532658" class="  clearfix">
				<p>From the APA Style <a href="https://apastyle.apa.org/style-grammar-guidelines/citations/personal-communications">section on Personal Communications</a>:</p>

<p>"To describe Traditional Knowledge or Oral Traditions that are not recorded (and therefore are not recoverable by readers), provide as much detail in the in-text citation as is necessary to describe the content and to contextualize the origin of the information. For example, if you spoke with an Indigenous person directly to learn information (but they were not a research participant), use a variation of the personal communication citation.</p>

<ul>
	<li>Provide the person’s full name and the nation or specific Indigenous group to which they belong, as well as their location or other details about them as relevant, followed by the words “personal communication,” and the date of the communication.</li>
	<li>Provide an exact date of correspondence if available; if correspondence took place over a period of time, provide a more general date or a range of dates. The date refers to when you consulted with the person, not to when the information originated.</li>
	<li>Ensure that the person agrees to have their name included in your paper and confirms the accuracy and appropriateness of the information you present.</li>
	<li>Because there is no recoverable source, a reference list entry is not used."</li>
</ul>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div><div id="s-lg-box-wrapper-35946581" class="s-lg-box-wrapper-35946581">
					<div id="s-lg-box-30561708-container" class="s-lib-box-container">
						<div id="s-lg-box-30561708" class="s-lib-box s-lib-box-std">
							<h2 class="s-lib-box-title">ChatGPT or Other Generative AI
                                </h2>
							<div id="s-lg-box-collapse-30561708">
								<div class="s-lib-box-content pad-left-none pad-right-none">
									
			<div id="s-lg-content-71362375" class="  clearfix">
				<p>APA issued preliminary guidance on an April&nbsp;7, 2023 blog post:&nbsp;<a href="https://apastyle.apa.org/blog/how-to-cite-chatgpt">How to Cite ChatGPT</a></p>

<h4>Quoting or reproducing the text created by ChatGPT in your paper</h4>

<p style="margin-left: 40px;">Remember that the results of a ChatGPT chat you did are not accessible by others. According to <a href="https://apastyle.apa.org/blog/how-to-cite-chatgpt">APA</a>, "Quoting ChatGPT’s text from a chat session is therefore more like sharing an algorithm’s output; thus, credit the author of the algorithm with a reference list entry and the corresponding in-text citation.</p>

<h4>In-text citation example from <a href="https://apastyle.apa.org/blog/how-to-cite-chatgpt">APA </a>blog</h4>

<p style="margin-left: 40px;">When prompted with “Is the left brain right brain divide real or a metaphor?” the ChatGPT-generated text indicated that although the two brain hemispheres are somewhat specialized, “the notation that people can be characterized as ‘left-brained’ or ‘right-brained’ is considered to be an oversimplification and a popular myth” (OpenAI, 2023).</p>

<h4>References page entry</h4>

<p>OpenAI. (2023).&nbsp;<em>ChatGPT</em>&nbsp;(Mar 14 version) [Large language model].&nbsp;https://chat.openai.com/chat</p>

		   </div>
								</div>
								
							</div>
						</div>
					</div></div>
          </div>
     </div>
     </div></div>
     </div>
</div>
                            </div>
                        </div>
            			<ul id="s-lg-page-prevnext" class="pager "><li class="previous"><a href="https://nwtc.libguides.com/c.php?g=29261&amp;p=182452">&lt;&lt; <strong>Previous:</strong> Home</a></li><li class="next"><a href="https://nwtc.libguides.com/c.php?g=29261&amp;p=9600724"><strong>Next:</strong> APA Formatting Tips &gt;&gt;</a></li></ul>
                    </div>
                </div>
<div class="border py-2 graph-before-activity-overview">
      <div class="js-calendar-graph mx-md-2 mx-3 d-flex flex-column flex-items-end flex-xl-items-center overflow-hidden pt-1 is-graph-loading graph-canvas ContributionCalendar height-full text-center" data-graph-url="/users/doZennn/contributions" data-url="/doZennn" data-from="2022-11-06 00:00:00 +0800" data-to="2023-11-08 23:59:59 +0800" data-org="">
        

<div style="max-width: 100%; overflow-y: hidden; overflow-x: auto">
  <table data-hydro-click="{&quot;event_type&quot;:&quot;user_profile.click&quot;,&quot;payload&quot;:{&quot;profile_user_id&quot;:10759579,&quot;target&quot;:&quot;CONTRIBUTION_CALENDAR_SQUARE&quot;,&quot;user_id&quot;:101152077,&quot;originating_url&quot;:&quot;https://github.com/doZennn&quot;}}" data-hydro-click-hmac="60cd6a83cfe3b2dfcb42c57984b450edbb95923aca80d9ee10374102ab6f441c" role="grid" aria-readonly="true" class="ContributionCalendar-grid js-calendar-graph-table" style="border-spacing: 3px; overflow: hidden; position: relative">
    <caption class="sr-only">Contribution Graph</caption>

    <thead>
      <tr style="height: 13px">
        <td style="width: 28px">
          <span class="sr-only">Day of Week</span>
        </td>


          <td class="ContributionCalendar-label" colspan="4" style="position: relative">
            <span class="sr-only">November</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Nov</span>
          </td>

          <td class="ContributionCalendar-label" colspan="4" style="position: relative">
            <span class="sr-only">December</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Dec</span>
          </td>

          <td class="ContributionCalendar-label" colspan="5" style="position: relative">
            <span class="sr-only">January</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Jan</span>
          </td>

          <td class="ContributionCalendar-label" colspan="4" style="position: relative">
            <span class="sr-only">February</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Feb</span>
          </td>

          <td class="ContributionCalendar-label" colspan="4" style="position: relative">
            <span class="sr-only">March</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Mar</span>
          </td>

          <td class="ContributionCalendar-label" colspan="5" style="position: relative">
            <span class="sr-only">April</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Apr</span>
          </td>

          <td class="ContributionCalendar-label" colspan="4" style="position: relative">
            <span class="sr-only">May</span>
            <span aria-hidden="true" style="position: absolute; top: 0">May</span>
          </td>

          <td class="ContributionCalendar-label" colspan="4" style="position: relative">
            <span class="sr-only">June</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Jun</span>
          </td>

          <td class="ContributionCalendar-label" colspan="5" style="position: relative">
            <span class="sr-only">July</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Jul</span>
          </td>

          <td class="ContributionCalendar-label" colspan="4" style="position: relative">
            <span class="sr-only">August</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Aug</span>
          </td>

          <td class="ContributionCalendar-label" colspan="4" style="position: relative">
            <span class="sr-only">September</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Sep</span>
          </td>

          <td class="ContributionCalendar-label" colspan="5" style="position: relative">
            <span class="sr-only">October</span>
            <span aria-hidden="true" style="position: absolute; top: 0">Oct</span>
          </td>
      </tr>
    </thead>

    <tbody>
        <tr style="height: 10px">
          <td class="ContributionCalendar-label" style="position: relative">
            <span class="sr-only">Sunday</span>
            <span aria-hidden="true" style="clip-path: Circle(0); position: absolute; bottom: -3px">
              Sun
            </span>
          </td>

              <td class="ContributionCalendar-day" tabindex="-1" data-ix="0" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-11-06" data-level="0"><span class="sr-only">No contributions on November 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="1" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-13" data-level="1"><span class="sr-only">3 contributions on November 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="2" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-20" data-level="1"><span class="sr-only">3 contributions on November 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="3" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-27" data-level="1"><span class="sr-only">3 contributions on November 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="4" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-04" data-level="1"><span class="sr-only">5 contributions on December 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="5" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-11" data-level="1"><span class="sr-only">6 contributions on December 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="6" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-12-18" data-level="0"><span class="sr-only">No contributions on December 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="7" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-12-25" data-level="0"><span class="sr-only">No contributions on December 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="8" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-01" data-level="0"><span class="sr-only">No contributions on January 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="9" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-08" data-level="1"><span class="sr-only">1 contribution on January 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="10" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-15" data-level="0"><span class="sr-only">No contributions on January 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="11" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-22" data-level="1"><span class="sr-only">2 contributions on January 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="12" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-29" data-level="0"><span class="sr-only">No contributions on January 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="13" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-05" data-level="1"><span class="sr-only">2 contributions on February 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="14" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-02-12" data-level="0"><span class="sr-only">No contributions on February 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="15" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-19" data-level="1"><span class="sr-only">6 contributions on February 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="16" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-26" data-level="1"><span class="sr-only">1 contribution on February 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="17" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-03-05" data-level="0"><span class="sr-only">No contributions on March 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="18" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-03-12" data-level="0"><span class="sr-only">No contributions on March 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="19" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-19" data-level="1"><span class="sr-only">7 contributions on March 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="20" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-26" data-level="1"><span class="sr-only">2 contributions on March 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="21" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-02" data-level="1"><span class="sr-only">14 contributions on April 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="22" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-09" data-level="1"><span class="sr-only">1 contribution on April 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="23" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-04-16" data-level="0"><span class="sr-only">No contributions on April 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="24" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-04-23" data-level="0"><span class="sr-only">No contributions on April 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="25" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-30" data-level="1"><span class="sr-only">2 contributions on April 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="26" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-07" data-level="0"><span class="sr-only">No contributions on May 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="27" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-14" data-level="1"><span class="sr-only">1 contribution on May 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="28" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-21" data-level="0"><span class="sr-only">No contributions on May 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="29" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-28" data-level="0"><span class="sr-only">No contributions on May 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="30" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-04" data-level="0"><span class="sr-only">No contributions on June 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="31" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-11" data-level="0"><span class="sr-only">No contributions on June 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="32" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-18" data-level="0"><span class="sr-only">No contributions on June 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="33" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-25" data-level="0"><span class="sr-only">No contributions on June 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="34" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-02" data-level="0"><span class="sr-only">No contributions on July 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="35" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-09" data-level="0"><span class="sr-only">No contributions on July 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="36" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-16" data-level="0"><span class="sr-only">No contributions on July 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="37" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-23" data-level="0"><span class="sr-only">No contributions on July 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="38" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-30" data-level="0"><span class="sr-only">No contributions on July 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="39" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-06" data-level="0"><span class="sr-only">No contributions on August 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="40" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-13" data-level="0"><span class="sr-only">No contributions on August 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="41" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-20" data-level="1"><span class="sr-only">13 contributions on August 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="42" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-27" data-level="0"><span class="sr-only">No contributions on August 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="43" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-03" data-level="0"><span class="sr-only">No contributions on September 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="44" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-10" data-level="0"><span class="sr-only">No contributions on September 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="45" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-17" data-level="1"><span class="sr-only">1 contribution on September 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="46" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-24" data-level="1"><span class="sr-only">3 contributions on September 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="47" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-01" data-level="0"><span class="sr-only">No contributions on October 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="48" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-08" data-level="0"><span class="sr-only">No contributions on October 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="49" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-15" data-level="0"><span class="sr-only">No contributions on October 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="50" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-22" data-level="0"><span class="sr-only">No contributions on October 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="51" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-10-29" data-level="1"><span class="sr-only">1 contribution on October 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="52" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-11-05" data-level="0"><span class="sr-only">No contributions on November 5th.</span></td>
        </tr>
        <tr style="height: 10px">
          <td class="ContributionCalendar-label" style="position: relative">
            <span class="sr-only">Monday</span>
            <span aria-hidden="true" style="clip-path: None; position: absolute; bottom: -3px">
              Mon
            </span>
          </td>

              <td class="ContributionCalendar-day" tabindex="-1" data-ix="0" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-07" data-level="1"><span class="sr-only">1 contribution on November 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="1" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-14" data-level="1"><span class="sr-only">11 contributions on November 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="2" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-21" data-level="1"><span class="sr-only">14 contributions on November 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="3" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-28" data-level="1"><span class="sr-only">2 contributions on November 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="4" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-05" data-level="1"><span class="sr-only">6 contributions on December 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="5" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-12" data-level="1"><span class="sr-only">10 contributions on December 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="6" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-19" data-level="1"><span class="sr-only">2 contributions on December 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="7" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-12-26" data-level="0"><span class="sr-only">No contributions on December 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="8" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-02" data-level="0"><span class="sr-only">No contributions on January 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="9" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-09" data-level="0"><span class="sr-only">No contributions on January 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="10" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-16" data-level="1"><span class="sr-only">1 contribution on January 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="11" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-23" data-level="1"><span class="sr-only">13 contributions on January 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="12" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-30" data-level="0"><span class="sr-only">No contributions on January 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="13" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-06" data-level="1"><span class="sr-only">2 contributions on February 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="14" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-02-13" data-level="0"><span class="sr-only">No contributions on February 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="15" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-02-20" data-level="0"><span class="sr-only">No contributions on February 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="16" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-02-27" data-level="0"><span class="sr-only">No contributions on February 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="0" data-ix="17" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-06" data-level="1"><span class="sr-only">5 contributions on March 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="18" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-03-13" data-level="0"><span class="sr-only">No contributions on March 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="19" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-20" data-level="1"><span class="sr-only">7 contributions on March 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="20" aria-selected="false" aria-describedby="contribution-graph-legend-level-3" style="width: 10px" data-date="2023-03-27" data-level="3"><span class="sr-only">65 contributions on March 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="21" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-03" data-level="1"><span class="sr-only">2 contributions on April 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="22" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-10" data-level="1"><span class="sr-only">2 contributions on April 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="23" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-17" data-level="1"><span class="sr-only">3 contributions on April 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="24" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-24" data-level="1"><span class="sr-only">6 contributions on April 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="25" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-01" data-level="1"><span class="sr-only">1 contribution on May 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="26" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-08" data-level="1"><span class="sr-only">4 contributions on May 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="27" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-15" data-level="0"><span class="sr-only">No contributions on May 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="28" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-22" data-level="0"><span class="sr-only">No contributions on May 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="29" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-29" data-level="1"><span class="sr-only">6 contributions on May 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="30" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-05" data-level="0"><span class="sr-only">No contributions on June 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="31" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-12" data-level="0"><span class="sr-only">No contributions on June 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="32" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-19" data-level="1"><span class="sr-only">4 contributions on June 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="33" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-26" data-level="0"><span class="sr-only">No contributions on June 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="34" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-07-03" data-level="1"><span class="sr-only">1 contribution on July 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="35" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-10" data-level="0"><span class="sr-only">No contributions on July 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="36" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-17" data-level="0"><span class="sr-only">No contributions on July 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="37" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-07-24" data-level="1"><span class="sr-only">1 contribution on July 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="38" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-31" data-level="0"><span class="sr-only">No contributions on July 31st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="39" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-07" data-level="0"><span class="sr-only">No contributions on August 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="40" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-14" data-level="1"><span class="sr-only">2 contributions on August 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="41" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-21" data-level="1"><span class="sr-only">1 contribution on August 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="42" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-28" data-level="1"><span class="sr-only">4 contributions on August 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="43" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-04" data-level="1"><span class="sr-only">3 contributions on September 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="44" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-11" data-level="0"><span class="sr-only">No contributions on September 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="45" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-18" data-level="1"><span class="sr-only">3 contributions on September 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="46" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-25" data-level="1"><span class="sr-only">6 contributions on September 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="47" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-10-02" data-level="1"><span class="sr-only">2 contributions on October 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="48" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-09" data-level="0"><span class="sr-only">No contributions on October 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="49" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-16" data-level="0"><span class="sr-only">No contributions on October 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="50" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-23" data-level="0"><span class="sr-only">No contributions on October 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="51" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-30" data-level="0"><span class="sr-only">No contributions on October 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="52" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-11-06" data-level="0"><span class="sr-only">No contributions on November 6th.</span></td>
        </tr>
        <tr style="height: 10px">
          <td class="ContributionCalendar-label" style="position: relative">
            <span class="sr-only">Tuesday</span>
            <span aria-hidden="true" style="clip-path: Circle(0); position: absolute; bottom: -3px">
              Tue
            </span>
          </td>

              <td class="ContributionCalendar-day" tabindex="-1" data-ix="0" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-11-08" data-level="0"><span class="sr-only">No contributions on November 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="1" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-15" data-level="1"><span class="sr-only">4 contributions on November 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="2" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-22" data-level="1"><span class="sr-only">5 contributions on November 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="3" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-29" data-level="1"><span class="sr-only">9 contributions on November 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="4" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-06" data-level="1"><span class="sr-only">12 contributions on December 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="5" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-13" data-level="1"><span class="sr-only">11 contributions on December 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="6" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-12-20" data-level="0"><span class="sr-only">No contributions on December 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="7" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-12-27" data-level="0"><span class="sr-only">No contributions on December 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="8" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-03" data-level="1"><span class="sr-only">1 contribution on January 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="9" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-10" data-level="1"><span class="sr-only">1 contribution on January 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="10" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-17" data-level="1"><span class="sr-only">1 contribution on January 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="11" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-24" data-level="1"><span class="sr-only">3 contributions on January 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="12" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-31" data-level="0"><span class="sr-only">No contributions on January 31st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="13" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-02-07" data-level="0"><span class="sr-only">No contributions on February 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="14" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-14" data-level="1"><span class="sr-only">4 contributions on February 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="15" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-21" data-level="1"><span class="sr-only">7 contributions on February 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="16" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-28" data-level="1"><span class="sr-only">10 contributions on February 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="17" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-03-07" data-level="0"><span class="sr-only">No contributions on March 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="18" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-14" data-level="1"><span class="sr-only">4 contributions on March 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="19" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-21" data-level="1"><span class="sr-only">13 contributions on March 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="20" aria-selected="false" aria-describedby="contribution-graph-legend-level-4" style="width: 10px" data-date="2023-03-28" data-level="4"><span class="sr-only">103 contributions on March 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="21" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-04" data-level="1"><span class="sr-only">2 contributions on April 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="22" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-11" data-level="1"><span class="sr-only">3 contributions on April 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="23" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-18" data-level="1"><span class="sr-only">5 contributions on April 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="24" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-04-25" data-level="0"><span class="sr-only">No contributions on April 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="25" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-02" data-level="0"><span class="sr-only">No contributions on May 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="26" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-09" data-level="1"><span class="sr-only">3 contributions on May 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="27" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-16" data-level="0"><span class="sr-only">No contributions on May 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="28" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-23" data-level="1"><span class="sr-only">2 contributions on May 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="29" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-30" data-level="1"><span class="sr-only">3 contributions on May 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="30" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-06" data-level="0"><span class="sr-only">No contributions on June 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="31" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-13" data-level="0"><span class="sr-only">No contributions on June 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="32" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-20" data-level="0"><span class="sr-only">No contributions on June 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="33" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-27" data-level="1"><span class="sr-only">2 contributions on June 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="34" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-04" data-level="0"><span class="sr-only">No contributions on July 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="35" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-11" data-level="0"><span class="sr-only">No contributions on July 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="36" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-18" data-level="0"><span class="sr-only">No contributions on July 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="37" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-07-25" data-level="1"><span class="sr-only">1 contribution on July 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="38" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-01" data-level="0"><span class="sr-only">No contributions on August 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="39" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-08" data-level="0"><span class="sr-only">No contributions on August 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="40" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-15" data-level="1"><span class="sr-only">10 contributions on August 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="41" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-22" data-level="1"><span class="sr-only">7 contributions on August 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="42" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-29" data-level="0"><span class="sr-only">No contributions on August 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="43" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-05" data-level="0"><span class="sr-only">No contributions on September 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="44" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-12" data-level="1"><span class="sr-only">5 contributions on September 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="45" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-19" data-level="0"><span class="sr-only">No contributions on September 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="46" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-26" data-level="1"><span class="sr-only">3 contributions on September 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="47" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-03" data-level="0"><span class="sr-only">No contributions on October 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="48" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-10" data-level="0"><span class="sr-only">No contributions on October 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="49" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-10-17" data-level="1"><span class="sr-only">2 contributions on October 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="50" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-10-24" data-level="1"><span class="sr-only">1 contribution on October 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="51" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-31" data-level="0"><span class="sr-only">No contributions on October 31st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="52" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-11-07" data-level="1"><span class="sr-only">2 contributions on November 7th.</span></td>
        </tr>
        <tr style="height: 10px">
          <td class="ContributionCalendar-label" style="position: relative">
            <span class="sr-only">Wednesday</span>
            <span aria-hidden="true" style="clip-path: None; position: absolute; bottom: -3px">
              Wed
            </span>
          </td>

              <td class="ContributionCalendar-day" tabindex="-1" data-ix="0" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-11-09" data-level="0"><span class="sr-only">No contributions on November 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="1" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-16" data-level="1"><span class="sr-only">2 contributions on November 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="2" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-23" data-level="1"><span class="sr-only">6 contributions on November 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="3" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-30" data-level="1"><span class="sr-only">11 contributions on November 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="4" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-07" data-level="1"><span class="sr-only">8 contributions on December 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="5" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-14" data-level="1"><span class="sr-only">4 contributions on December 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="6" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-12-21" data-level="0"><span class="sr-only">No contributions on December 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="7" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-28" data-level="1"><span class="sr-only">1 contribution on December 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="8" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-04" data-level="0"><span class="sr-only">No contributions on January 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="9" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-11" data-level="1"><span class="sr-only">1 contribution on January 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="10" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-18" data-level="1"><span class="sr-only">1 contribution on January 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="11" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-25" data-level="0"><span class="sr-only">No contributions on January 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="12" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-01" data-level="1"><span class="sr-only">1 contribution on February 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="13" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-08" data-level="1"><span class="sr-only">5 contributions on February 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="14" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-15" data-level="1"><span class="sr-only">2 contributions on February 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="15" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-22" data-level="1"><span class="sr-only">3 contributions on February 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="16" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-03-01" data-level="0"><span class="sr-only">No contributions on March 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="17" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-08" data-level="1"><span class="sr-only">3 contributions on March 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="18" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-03-15" data-level="0"><span class="sr-only">No contributions on March 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="19" aria-selected="false" aria-describedby="contribution-graph-legend-level-2" style="width: 10px" data-date="2023-03-22" data-level="2"><span class="sr-only">46 contributions on March 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="20" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-29" data-level="1"><span class="sr-only">12 contributions on March 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="21" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-05" data-level="1"><span class="sr-only">7 contributions on April 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="22" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-12" data-level="1"><span class="sr-only">6 contributions on April 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="23" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-04-19" data-level="0"><span class="sr-only">No contributions on April 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="24" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-04-26" data-level="0"><span class="sr-only">No contributions on April 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="25" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-03" data-level="1"><span class="sr-only">3 contributions on May 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="26" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-10" data-level="1"><span class="sr-only">1 contribution on May 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="27" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-17" data-level="0"><span class="sr-only">No contributions on May 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="28" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-24" data-level="0"><span class="sr-only">No contributions on May 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="29" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-31" data-level="1"><span class="sr-only">1 contribution on May 31st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="30" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-07" data-level="0"><span class="sr-only">No contributions on June 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="31" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-14" data-level="1"><span class="sr-only">1 contribution on June 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="32" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-21" data-level="1"><span class="sr-only">1 contribution on June 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="33" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-28" data-level="0"><span class="sr-only">No contributions on June 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="34" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-05" data-level="0"><span class="sr-only">No contributions on July 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="35" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-07-12" data-level="1"><span class="sr-only">1 contribution on July 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="36" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-19" data-level="0"><span class="sr-only">No contributions on July 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="37" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-07-26" data-level="1"><span class="sr-only">2 contributions on July 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="38" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-02" data-level="0"><span class="sr-only">No contributions on August 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="39" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-09" data-level="1"><span class="sr-only">4 contributions on August 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="40" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-16" data-level="1"><span class="sr-only">1 contribution on August 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="41" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-23" data-level="1"><span class="sr-only">1 contribution on August 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="42" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-30" data-level="1"><span class="sr-only">2 contributions on August 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="43" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-06" data-level="1"><span class="sr-only">3 contributions on September 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="44" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-13" data-level="0"><span class="sr-only">No contributions on September 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="45" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-20" data-level="1"><span class="sr-only">3 contributions on September 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="46" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-27" data-level="0"><span class="sr-only">No contributions on September 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="47" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-04" data-level="0"><span class="sr-only">No contributions on October 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="48" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-11" data-level="0"><span class="sr-only">No contributions on October 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="49" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-18" data-level="0"><span class="sr-only">No contributions on October 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="50" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-10-25" data-level="1"><span class="sr-only">1 contribution on October 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="51" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-11-01" data-level="0"><span class="sr-only">No contributions on November 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="52" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-11-08" data-level="1"><span class="sr-only">7 contributions on November 8th.</span></td>
        </tr>
        <tr style="height: 10px">
          <td class="ContributionCalendar-label" style="position: relative">
            <span class="sr-only">Thursday</span>
            <span aria-hidden="true" style="clip-path: Circle(0); position: absolute; bottom: -3px">
              Thu
            </span>
          </td>

              <td class="ContributionCalendar-day" tabindex="-1" data-ix="0" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-11-10" data-level="0"><span class="sr-only">No contributions on November 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="1" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-17" data-level="1"><span class="sr-only">3 contributions on November 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="2" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-24" data-level="1"><span class="sr-only">6 contributions on November 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="3" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-01" data-level="1"><span class="sr-only">3 contributions on December 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="4" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-08" data-level="1"><span class="sr-only">3 contributions on December 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="5" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-15" data-level="1"><span class="sr-only">4 contributions on December 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="6" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-22" data-level="1"><span class="sr-only">1 contribution on December 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="7" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-29" data-level="1"><span class="sr-only">6 contributions on December 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="8" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-05" data-level="0"><span class="sr-only">No contributions on January 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="9" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-12" data-level="0"><span class="sr-only">No contributions on January 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="10" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-19" data-level="1"><span class="sr-only">3 contributions on January 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="11" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-26" data-level="0"><span class="sr-only">No contributions on January 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="12" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-02-02" data-level="0"><span class="sr-only">No contributions on February 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="13" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-09" data-level="1"><span class="sr-only">1 contribution on February 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="14" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-16" data-level="1"><span class="sr-only">7 contributions on February 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="15" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-23" data-level="1"><span class="sr-only">5 contributions on February 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="16" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-02" data-level="1"><span class="sr-only">2 contributions on March 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="17" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-09" data-level="1"><span class="sr-only">2 contributions on March 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="18" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-16" data-level="1"><span class="sr-only">1 contribution on March 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="19" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-23" data-level="1"><span class="sr-only">1 contribution on March 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="20" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-30" data-level="1"><span class="sr-only">10 contributions on March 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="21" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-06" data-level="1"><span class="sr-only">3 contributions on April 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="22" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-04-13" data-level="0"><span class="sr-only">No contributions on April 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="23" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-20" data-level="1"><span class="sr-only">4 contributions on April 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="24" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-27" data-level="1"><span class="sr-only">7 contributions on April 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="25" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-04" data-level="1"><span class="sr-only">1 contribution on May 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="26" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-11" data-level="1"><span class="sr-only">3 contributions on May 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="27" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-18" data-level="0"><span class="sr-only">No contributions on May 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="28" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-25" data-level="0"><span class="sr-only">No contributions on May 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="29" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-01" data-level="1"><span class="sr-only">3 contributions on June 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="30" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-08" data-level="0"><span class="sr-only">No contributions on June 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="31" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-15" data-level="1"><span class="sr-only">3 contributions on June 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="32" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-22" data-level="0"><span class="sr-only">No contributions on June 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="33" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-29" data-level="0"><span class="sr-only">No contributions on June 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="34" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-06" data-level="0"><span class="sr-only">No contributions on July 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="35" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-13" data-level="0"><span class="sr-only">No contributions on July 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="36" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-20" data-level="0"><span class="sr-only">No contributions on July 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="37" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-07-27" data-level="1"><span class="sr-only">1 contribution on July 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="38" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-03" data-level="1"><span class="sr-only">8 contributions on August 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="39" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-10" data-level="1"><span class="sr-only">1 contribution on August 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="40" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-17" data-level="1"><span class="sr-only">1 contribution on August 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="41" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-24" data-level="1"><span class="sr-only">5 contributions on August 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="42" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-08-31" data-level="1"><span class="sr-only">2 contributions on August 31st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="43" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-07" data-level="0"><span class="sr-only">No contributions on September 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="44" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-14" data-level="0"><span class="sr-only">No contributions on September 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="45" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-21" data-level="1"><span class="sr-only">2 contributions on September 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="46" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-28" data-level="0"><span class="sr-only">No contributions on September 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="47" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-05" data-level="0"><span class="sr-only">No contributions on October 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="48" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-12" data-level="0"><span class="sr-only">No contributions on October 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="49" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-19" data-level="0"><span class="sr-only">No contributions on October 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="50" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-26" data-level="0"><span class="sr-only">No contributions on October 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="51" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-11-02" data-level="1"><span class="sr-only">1 contribution on November 2nd.</span></td>
              <td></td>
        </tr>
        <tr style="height: 10px">
          <td class="ContributionCalendar-label" style="position: relative">
            <span class="sr-only">Friday</span>
            <span aria-hidden="true" style="clip-path: None; position: absolute; bottom: -3px">
              Fri
            </span>
          </td>

              <td class="ContributionCalendar-day" tabindex="-1" data-ix="0" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-11" data-level="1"><span class="sr-only">4 contributions on November 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="1" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-18" data-level="1"><span class="sr-only">8 contributions on November 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="2" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-11-25" data-level="0"><span class="sr-only">No contributions on November 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="3" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-02" data-level="1"><span class="sr-only">9 contributions on December 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="4" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-09" data-level="1"><span class="sr-only">10 contributions on December 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="5" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-16" data-level="1"><span class="sr-only">6 contributions on December 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="6" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-23" data-level="1"><span class="sr-only">1 contribution on December 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="7" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-12-30" data-level="0"><span class="sr-only">No contributions on December 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="8" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-06" data-level="0"><span class="sr-only">No contributions on January 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="9" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-13" data-level="0"><span class="sr-only">No contributions on January 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="10" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-20" data-level="1"><span class="sr-only">2 contributions on January 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="11" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-27" data-level="1"><span class="sr-only">1 contribution on January 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="12" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-03" data-level="1"><span class="sr-only">2 contributions on February 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="13" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-02-10" data-level="0"><span class="sr-only">No contributions on February 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="14" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-17" data-level="1"><span class="sr-only">1 contribution on February 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="15" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-24" data-level="1"><span class="sr-only">4 contributions on February 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="16" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-03" data-level="1"><span class="sr-only">8 contributions on March 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="17" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-03-10" data-level="0"><span class="sr-only">No contributions on March 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="18" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-17" data-level="1"><span class="sr-only">1 contribution on March 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="19" aria-selected="false" aria-describedby="contribution-graph-legend-level-2" style="width: 10px" data-date="2023-03-24" data-level="2"><span class="sr-only">32 contributions on March 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="20" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-31" data-level="1"><span class="sr-only">11 contributions on March 31st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="21" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-04-07" data-level="0"><span class="sr-only">No contributions on April 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="22" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-14" data-level="1"><span class="sr-only">4 contributions on April 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="23" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-04-21" data-level="0"><span class="sr-only">No contributions on April 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="24" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-28" data-level="1"><span class="sr-only">1 contribution on April 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="25" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-05" data-level="0"><span class="sr-only">No contributions on May 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="26" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-12" data-level="1"><span class="sr-only">1 contribution on May 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="27" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-19" data-level="0"><span class="sr-only">No contributions on May 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="28" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-26" data-level="1"><span class="sr-only">5 contributions on May 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="29" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-02" data-level="1"><span class="sr-only">1 contribution on June 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="30" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-09" data-level="0"><span class="sr-only">No contributions on June 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="31" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-16" data-level="1"><span class="sr-only">14 contributions on June 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="32" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-23" data-level="1"><span class="sr-only">2 contributions on June 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="33" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-30" data-level="0"><span class="sr-only">No contributions on June 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="34" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-07" data-level="0"><span class="sr-only">No contributions on July 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="35" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-14" data-level="0"><span class="sr-only">No contributions on July 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="36" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-21" data-level="0"><span class="sr-only">No contributions on July 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="37" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-28" data-level="0"><span class="sr-only">No contributions on July 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="38" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-04" data-level="0"><span class="sr-only">No contributions on August 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="39" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-11" data-level="0"><span class="sr-only">No contributions on August 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="40" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-18" data-level="0"><span class="sr-only">No contributions on August 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="41" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-25" data-level="0"><span class="sr-only">No contributions on August 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="42" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-01" data-level="0"><span class="sr-only">No contributions on September 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="43" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-08" data-level="0"><span class="sr-only">No contributions on September 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="44" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-15" data-level="0"><span class="sr-only">No contributions on September 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="45" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-22" data-level="0"><span class="sr-only">No contributions on September 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="46" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-29" data-level="1"><span class="sr-only">1 contribution on September 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="47" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-06" data-level="0"><span class="sr-only">No contributions on October 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="48" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-13" data-level="0"><span class="sr-only">No contributions on October 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="49" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-10-20" data-level="1"><span class="sr-only">1 contribution on October 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="50" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-27" data-level="0"><span class="sr-only">No contributions on October 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="51" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-11-03" data-level="1"><span class="sr-only">1 contribution on November 3rd.</span></td>
              <td></td>
        </tr>
        <tr style="height: 10px">
          <td class="ContributionCalendar-label" style="position: relative">
            <span class="sr-only">Saturday</span>
            <span aria-hidden="true" style="clip-path: Circle(0); position: absolute; bottom: -3px">
              Sat
            </span>
          </td>

              <td class="ContributionCalendar-day" tabindex="-1" data-ix="0" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-12" data-level="1"><span class="sr-only">3 contributions on November 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="1" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-19" data-level="1"><span class="sr-only">1 contribution on November 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="2" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-11-26" data-level="1"><span class="sr-only">3 contributions on November 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="3" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-03" data-level="1"><span class="sr-only">2 contributions on December 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="4" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-12-10" data-level="0"><span class="sr-only">No contributions on December 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="5" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2022-12-17" data-level="1"><span class="sr-only">4 contributions on December 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="6" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-12-24" data-level="0"><span class="sr-only">No contributions on December 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="7" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2022-12-31" data-level="0"><span class="sr-only">No contributions on December 31st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="8" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-07" data-level="0"><span class="sr-only">No contributions on January 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="9" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-01-14" data-level="0"><span class="sr-only">No contributions on January 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="10" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-21" data-level="1"><span class="sr-only">5 contributions on January 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="11" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-01-28" data-level="1"><span class="sr-only">1 contribution on January 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="12" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-02-04" data-level="0"><span class="sr-only">No contributions on February 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="13" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-02-11" data-level="0"><span class="sr-only">No contributions on February 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="14" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-02-18" data-level="1"><span class="sr-only">2 contributions on February 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="15" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-02-25" data-level="0"><span class="sr-only">No contributions on February 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="16" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-04" data-level="1"><span class="sr-only">4 contributions on March 4th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="17" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-11" data-level="1"><span class="sr-only">5 contributions on March 11th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="18" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-18" data-level="1"><span class="sr-only">8 contributions on March 18th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="19" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-03-25" data-level="1"><span class="sr-only">6 contributions on March 25th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="20" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-01" data-level="1"><span class="sr-only">6 contributions on April 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="21" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-08" data-level="1"><span class="sr-only">3 contributions on April 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="22" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-04-15" data-level="1"><span class="sr-only">4 contributions on April 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="23" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-04-22" data-level="0"><span class="sr-only">No contributions on April 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="24" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-04-29" data-level="0"><span class="sr-only">No contributions on April 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="25" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-06" data-level="0"><span class="sr-only">No contributions on May 6th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="26" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-13" data-level="1"><span class="sr-only">1 contribution on May 13th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="27" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-05-20" data-level="0"><span class="sr-only">No contributions on May 20th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="28" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-05-27" data-level="1"><span class="sr-only">1 contribution on May 27th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="29" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-03" data-level="1"><span class="sr-only">1 contribution on June 3rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="30" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-10" data-level="0"><span class="sr-only">No contributions on June 10th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="31" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-06-17" data-level="1"><span class="sr-only">1 contribution on June 17th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="32" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-06-24" data-level="0"><span class="sr-only">No contributions on June 24th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="33" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-01" data-level="0"><span class="sr-only">No contributions on July 1st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="34" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-08" data-level="0"><span class="sr-only">No contributions on July 8th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="35" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-15" data-level="0"><span class="sr-only">No contributions on July 15th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="36" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-22" data-level="0"><span class="sr-only">No contributions on July 22nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="37" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-07-29" data-level="0"><span class="sr-only">No contributions on July 29th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="38" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-05" data-level="0"><span class="sr-only">No contributions on August 5th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="39" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-12" data-level="0"><span class="sr-only">No contributions on August 12th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="40" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-19" data-level="0"><span class="sr-only">No contributions on August 19th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="41" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-08-26" data-level="0"><span class="sr-only">No contributions on August 26th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="42" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-02" data-level="0"><span class="sr-only">No contributions on September 2nd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="43" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-09-09" data-level="0"><span class="sr-only">No contributions on September 9th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="44" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-16" data-level="1"><span class="sr-only">5 contributions on September 16th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="45" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-23" data-level="1"><span class="sr-only">3 contributions on September 23rd.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="46" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-09-30" data-level="1"><span class="sr-only">1 contribution on September 30th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="47" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-10-07" data-level="1"><span class="sr-only">1 contribution on October 7th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="48" aria-selected="false" aria-describedby="contribution-graph-legend-level-0" style="width: 10px" data-date="2023-10-14" data-level="0"><span class="sr-only">No contributions on October 14th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="49" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-10-21" data-level="1"><span class="sr-only">1 contribution on October 21st.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="50" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-10-28" data-level="1"><span class="sr-only">1 contribution on October 28th.</span></td>
              <td class="ContributionCalendar-day" tabindex="-1" data-ix="51" aria-selected="false" aria-describedby="contribution-graph-legend-level-1" style="width: 10px" data-date="2023-11-04" data-level="1"><span class="sr-only">1 contribution on November 4th.</span></td>
              <td></td>
        </tr>
    </tbody>
</table></div>

        <div class="width-full f6 px-0 px-md-5 py-1">
          <div class="float-left">


            <a href="https://docs.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile" class="Link--muted">
              Learn how we count contributions</a>
          </div>

            <div class="float-right color-fg-muted d-flex flex-items-center">
              <span data-view-component="true" class="mr-1">Less</span>
                <div style="width: 10px; height: 10px" id="contribution-graph-legend-level-0" data-level="0" data-view-component="true" class="ContributionCalendar-day rounded-1 mr-1"><span class="sr-only">No contributions.</span></div>
                <div style="width: 10px; height: 10px" id="contribution-graph-legend-level-1" data-level="1" data-view-component="true" class="ContributionCalendar-day rounded-1 mr-1"><span class="sr-only">Low contributions.</span></div>
                <div style="width: 10px; height: 10px" id="contribution-graph-legend-level-2" data-level="2" data-view-component="true" class="ContributionCalendar-day rounded-1 mr-1"><span class="sr-only">Medium-low contributions.</span></div>
                <div style="width: 10px; height: 10px" id="contribution-graph-legend-level-3" data-level="3" data-view-component="true" class="ContributionCalendar-day rounded-1 mr-1"><span class="sr-only">Medium-high contributions.</span></div>
                <div style="width: 10px; height: 10px" id="contribution-graph-legend-level-4" data-level="4" data-view-component="true" class="ContributionCalendar-day rounded-1 mr-1"><span class="sr-only">High contributions.</span></div>
              <span data-view-component="true">More</span>
            </div>
        </div>
      </div>

    </div><div id="file-sgdb_flashpoint_assignments-md" class="file my-2">
      <div id="file-sgdb_flashpoint_assignments-md-readme" class="Box-body readme blob js-code-block-container p-5 p-xl-6 gist-border-0">
    <article class="markdown-body entry-content container-lg" itemprop="text"><table>
<thead>
<tr>
<th>Game</th>
<th>Flashpoint GUID</th>
</tr>
</thead>
<tbody>
<tr>
<td><a href="https://www.steamgriddb.com/game/5844" rel="nofollow">1Quest</a></td>
<td><code>6de6ce3c-18c6-3015-e757-eb4f4b6f99ac</code></td>
</tr>
<tr>
<td><a href="https://www.steamgriddb.com/game/11903" rel="nofollow">4 Elements</a></td>
<td><code>23c02784-b1e6-8d75-e1fd-801e1e54db38</code></td>
</tr>
<tr>
<td><a href="https://www.steamgriddb.com/game/37200" rel="nofollow">Adventure</a></td>
<td><code>ee5f29d0-2336-9f4d-6071-3cbe28439829</code></td>
</tr>
<tr>
<td><a href="https://www.steamgriddb.com/game/5924" rel="nofollow">Adventures of Robinson Crusoe</a></td>
<td><code>4d3b812f-2305-41a8-b808-547ab2c82518</code></td>
</tr>
<tr>
<td><a href="https://www.steamgriddb.com/game/5276900" rel="nofollow">Airport Mania: First Flight</a></td>
<td><code>d5a31036-61f7-411b-8cde-ae53d2db608c</code></td>
</tr>
<tr>
<td><a href="https://www.steamgriddb.com/game/14699" rel="nofollow">Alchemist</a></td>
<td><code>6e842d2b-d65d-44b9-9c9b-bcec42409d49</code></td>
</tr>
<tr>
<td><a href="https://www.steamgriddb.com/game/26177" rel="nofollow">Alice in Wonderland - Hidden Objects</a></td>
<td><code>c670f1fe-c0af-5c9c-21c1-762df244a5a0</code></td>
</tr>
<tr>
<td><a href="https://www.steamgriddb.com/game/9200" rel="nofollow">All My Gods</a></td>
<td><code>2f579653-bfc7-4454-9163-3d0cde42a5d1</code></td>
</tr>
</tbody>
</table>
</article>
  </div>

  </div>
